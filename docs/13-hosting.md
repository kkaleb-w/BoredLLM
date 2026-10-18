# 13. Hosting

The creature must run continuously for the length of the study with no gaps. This document
lists the options. **Option 0 is the chosen one.** Prices are point-in-time list prices and
move, so confirm in the console before committing.

## Option 0 (chosen): AWS EC2

Decided. Kaleb runs this on EC2. Figures below are us-east-1 on-demand list prices, checked
2026-10-12.

### Pick an instance

| Instance | vCPU | RAM | Arch | On-demand | Per month |
|---|---|---|---|---|---|
| **t4g.medium** | 2 | 4 GiB | arm64 (Graviton2) | $0.0336/hr | about $24.53 |
| t4g.large | 2 | 8 GiB | arm64 | $0.0672/hr | about $49.06 |
| t3.medium | 2 | 4 GiB | x86_64 | $0.0416/hr | about $30.37 |
| c7g.large | 2 | 4 GiB | arm64 | $0.0725/hr | about $52.93 |
| t4g.xlarge | 4 | 16 GiB | arm64 | $0.1344/hr | about $98.11 |

**Recommendation: t4g.medium**, with one important design consequence.

### Why a burstable instance is actually correct here

The usual objection to the T family is CPU credits: sustained load drains them and the instance
throttles to baseline. That objection does not apply to this workload, and the reason is the
tick rate. At one tick per ten minutes, with a few hundred tokens generated per tick, a small
model runs for tens of seconds out of every six hundred. That is on the order of **five percent
duty cycle**. The load is spiky and low, which is exactly what burstable instances are built
for. Reserving a dedicated-vCPU instance for a five percent duty cycle is wasted money.

**Corollary: do not shorten the tick interval casually.** A five minute tick doubles the corpus
requirement and doubles the CPU duty cycle, and it is the fastest way to turn a cheap instance
into a throttled one.

### The memory trick that makes 4 GiB enough

Do **not** run three separate model servers for the three creatures. Run **one model process
and three creature states**: one set of weights in memory, three separate contexts, drives, and
memory stores. The creatures share weights and differ only in state, which is the correct
architecture anyway and it is what makes the smallest instance viable.

With that, three 0.5B to 1.5B models at 4-bit fit comfortably in 4 GiB. Without it, 8 GiB.

### Storage

A separate EBS volume for the frozen corpus, the memory databases, and the logs. The corpus is
the big item (a BrowseComp-Plus style fixed corpus is about 1.1 GB). A 20 GiB gp3 volume is
ample and costs on the order of a dollar or two a month at list price. **Never put the state on
instance storage**, because the state is the experiment.

### Two caveats

- **arm64.** t4g and c7g are Graviton, so use arm64 builds of Ollama and llama.cpp. Both
  provide them. If a dependency only ships x86_64, fall back to t3.
- **Spot.** t3.medium and t4g.medium spot run around $0.016 to $0.017 per hour, roughly half
  price, but the instance can be reclaimed. That is survivable **only** if state is written
  durably on every tick, which the design already requires.

## Option 1: Oracle Cloud Always Free (free, forever)

The best free option for an always-on small workload.

- **What you get (as of 2026):** Ampere A1 (ARM) compute, **2 OCPUs and 12 GB RAM** total,
  plus up to 200 GB of block storage. Free with no time limit, subject to capacity
  availability in your home region.
- **Important change:** Oracle **halved this allowance on 15 June 2026**, from the previous
  4 OCPUs and 24 GB to the current 2 OCPUs and 12 GB, without public announcement. If you
  find older guides promising 4 OCPU / 24 GB, they predate the cut. Free-tier instances over
  the new limit were shut down until resized.
- **Fitness for this project:** Excellent for CPU-only inference of a 0.5B to 1B model at a
  slow tick interval. 12 GB of RAM is generous for the model plus SQLite plus logs.
- **Caveats:** Capacity for Always Free A1 instances is not guaranteed and provisioning can
  fail in busy regions; retry or choose a different home region. ARM means you need ARM builds
  of your runtime (Ollama and llama.cpp both provide them).

## Option 2: Hetzner Cloud (cheap paid, very reliable)

The best value if you want a real machine with predictable behavior and no free-tier roulette.

- **Pricing (2026, Germany/Finland, ex VAT and ex IPv4):**
  - **CX23**: 2 vCPU, 4 GB RAM, 40 GB SSD, 20 TB traffic, about **€5.49/month** (roughly
    $7/month including an IPv4 address).
  - **CX33**: 4 vCPU, 8 GB RAM, 80 GB NVMe, 20 TB traffic, about **€8.49/month** (roughly
    $10.50/month including IPv4).
- **Note:** Hetzner raised prices on 15 June 2026 (CX line up roughly 31 to 38 percent) and
  some older plans (CX22, CX32) are now deprecated in favor of CX23 and CX33. Buy the current
  plans.
- **Fitness for this project:** CX33 (4 vCPU, 8 GB) is the comfortable pick. Four cores run a
  small model and the world simulation at a relaxed tick with room to spare. Hourly billing
  with a monthly cap means a short experiment costs only the hours it runs.
- **Caveats:** Shared vCPU, so throughput varies with neighbor load. Fine for this workload,
  not for latency-sensitive work.

## Option 3: Rented GPU by the hour (Vast.ai or RunPod)

If you want a real GPU, for faster ticks, larger models, or the weight-updating sleep.

- **Realistic rates (October 2026 snapshot):**
  - 24 GB cards (RTX 3090): from about **$0.12 to $0.22 per hour** depending on host and tier.
  - RTX 4090: about **$0.34/hour** on RunPod Community Cloud, from about **$0.14/hour** on
    Vast.ai's cheapest verified offers.
  - Interruptible Vast.ai listings run often 50 percent or more below on-demand, but the
    machine can be reclaimed, so only use them for workloads that can pause and resume.
- **Cost of a run:** Twenty days continuous on a 24 GB card at roughly $0.12 to $0.22/hour is
  about **$58 to $105**. On cheaper interruptible listings it can be substantially less,
  around $35.
- **Fitness for this project:** Overkill for a 1B model. The GPU is worth it only if you want
  faster ticks, a larger model, or on-device weight updates during sleep.
- **Caveats:** Prices are a marketplace and move hourly. Vast.ai charges bandwidth per host.
  Both bill per second. RunPod has fixed list prices and no data-transfer fees; Vast.ai has
  lower floors but more variance and host-specific risks (reliability, disk and bandwidth
  rates).

## A rough decision guide

| Goal | Pick |
|---|---|
| **Already on AWS, want the decided option** | **EC2 t4g.medium** (Option 0) |
| Zero budget, happy to provision ARM and retry on capacity | **Oracle Always Free** |
| Want it to just work, cheap, no surprises, non-AWS | **Hetzner CX33** |
| Want speed, a bigger model, or weight updates during sleep | **Vast.ai or RunPod**, short runs only |

## Deployment shape (any option)

- A single container, one state volume, a restart policy so it survives reboots.
- A wake routine on container start and a sleep routine on termination, so state is always
  written cleanly. See [Sleep](07-sleep-and-consolidation.md).
- Logs and the memory database on the durable volume from tick one. The logs are the
  experiment. See [The Experiment](09-the-experiment.md).
- No secrets needed on the machine if the model is local and the only external call is a
  keyless weather API. That is a real advantage of this design: nothing to leak.

## Related reading

- [12. Tools](12-tools.md) for what runs on the machine
- [09. The Experiment](09-the-experiment.md) for what the run is for
