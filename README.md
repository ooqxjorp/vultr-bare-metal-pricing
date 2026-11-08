# Vultr Bare Metal: How It's Priced, Which GPU Fits, and What to Check Before You Deploy (With Priced Alternatives)

Vultr Bare Metal is a single-tenant dedicated server with no hypervisor between your workload and the hardware, and Vultr sells it by configuration rather than from a fixed price table. That turns the first question into a harder one than it should be. The entry configuration (4 cores, 32 GB RAM, 2×240 GB SSD, 5 TB of transfer) shows up in third-party price tracking at **$120/month or $0.164/hour**; anything larger is quoted per build.

The same four cores and 32 GB with 1 TB of NVMe and 30 TB of transfer is listed at **$79/month** by SpinServers in Dallas. Which one is cheaper depends on what you're buying: hourly elasticity, 34 regions and a GPU catalogue on one side, storage and bandwidth per dollar on the other.

> **Quick answer:** for a CPU workload that runs month after month (app server, database, game server, VPN endpoint, CI runner), the 4-core/32 GB class is the sane entry point. SpinServers' Dallas Xeon E3-1280 v5 with 32 GB ECC, 1 TB NVMe and 30 TB on a 10 Gbps port starts at **$79/month**, roughly $41 below the tracked entry price for Vultr's comparable plan. 👉 [Check the Dallas 4-core/32 GB listing and today's price](https://bit.ly/Spinservers)

## Every current SpinServers bare metal plan, with prices

Vultr doesn't publish a fixed list to compare against, so the practical way to anchor a number is a real one. These are the Dallas and San Jose listings as they currently appear: monthly billing, instant automated provisioning, 30 TB of monthly transfer on 10 Gbps ports.

| Configuration | Location | CPU | RAM / storage | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- | --- |
| Intel Xeon E3-1280 v5, 1 TB NVMe | Dallas, TX | 4C/8T, 3.7 GHz turbo | 32 GB DDR4 ECC / 1 TB M.2 NVMe | **$79** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630L v3, 1 TB NVMe | Dallas, TX | 16C/32T, 2.90 GHz | 128 GB DDR4 ECC / 1 TB NVMe | Not shown | [Order](https://bit.ly/Spinservers) |
| Dual Xeon Gold 6148, 2×1.92 TB SSD | Dallas, TX | 40C/80T, 3.70 GHz | 256 GB DDR4 / 2×1.92 TB SSD | **$235** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon Platinum 8173M, 4×1.92 TB SSD | Dallas, TX | 56C/112T, 3.50 GHz | 512 GB DDR4 / 4×1.92 TB SSD | **$390** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630 v2 (SJC-207) | San Jose, CA | 12C/24T, 2.6–3.1 GHz | 64 GB DDR3 ECC / 240 GB SSD | **$55** | [Order](https://bit.ly/Spinservers) |
| Intel Xeon E3-1280 v5 (SJC-205) | San Jose, CA | 4C/8T, 3.7–4.0 GHz | 32 GB DDR4 ECC / 1 TB NVMe | **$79** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630L v3 (SJC-202) | San Jose, CA | 16C/32T, 1.8–2.9 GHz | 64 GB DDR4 ECC / 1.6 TB SSD | **$115** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630L v3 (SJC-206) | San Jose, CA | 16C/32T, 1.8–2.9 GHz | 64 GB DDR4 ECC / 1.6 TB NVMe | **$155** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2650L v3 (SJC-203) | San Jose, CA | 24C/48T, 1.8–2.5 GHz | 64 GB DDR4 ECC / 1.6 TB SSD | **$225** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630L v3 (SJC-201) | San Jose, CA | 16C/32T, 1.8–2.9 GHz | 256 GB DDR4 ECC / 2×1.6 TB SSD, hardware RAID with cache | **$225** | [Order](https://bit.ly/Spinservers) |
| Dual Xeon E5-2683 v4 (SJC-204) | San Jose, CA | 32C/64T, 2.1–3.0 GHz | 512 GB DDR4 ECC / 2×3.84 TB SSD, hardware RAID, battery-backed cache | **$250** | [Order](https://bit.ly/Spinservers) |

Three details that matter more than the headline price:

- The Dallas E3-1280 v5 and most San Jose configurations include 1 IPv4 address. The Dallas Gold 6148 and Platinum 8173M cards list 2×10 Gbps ports with LACP; San Jose cards list 2×10 Gbps ports as standard.
- The Dallas 128 GB dual E5-2630L v3 appears without a public starting price, so it sits unpriced in the table. The 256 GB Dallas box at $235 is the documented next step up.
- These are "starting from" prices billed monthly. Renewal terms and overage charges for traffic beyond 30 TB aren't shown in the listings, so confirm both before committing to a long workload.

## What Vultr Bare Metal actually is

Vultr Bare Metal is a single-tenant dedicated cloud server that runs no virtualization layer, with an operating system installed so applications sit directly on the hardware. A VPS or cloud instance shares physical hosts and divides them through a hypervisor; bare metal hands over the entire machine, which removes virtualization overhead and the variance that comes from other tenants on the same host. Vultr frames the benefits as maximum CPU, RAM and disk I/O efficiency, full control of the OS and software stack, a smaller exposure surface from single tenancy, and fewer unpredictable latency spikes.

The same inventory is reachable four ways: the console, the API, the CLI and the Terraform provider. The API call takes `region`, `plan`, `os_id`, `label` and `hostname`. Terraform (provider 2.26 or later) uses those fields with plan IDs such as `vbm-4c-32gb` and OS images such as `2284` for Ubuntu 24.04 x64, and returns the assigned `main_ip`. Vultr lists 34 cloud data center regions, DDoS mitigation against layer 3 and layer 4 attacks, BGP announcement of your own IP space, and attachments to its file system through storage gateways or to S3-compatible object storage.

Deployment speed is quoted two ways on Vultr's own pages: "in seconds" on the product page, and "some servers may be ready in minutes" in the FAQ. Budget in minutes, especially for GPU configurations.

## How Vultr's bare metal pricing works

No Vultr page gives a monthly table. The FAQ states that pricing depends on hardware configuration, network bandwidth and additional services, and mentions on-demand and reserved options without figures. For budgeting, the useful public data point is third-party tracking of the entry plan: 4 cores, 32 GB RAM, 2×240 GB SSD and 5 TB of bandwidth at **$120/month or $0.164/hour** across Amsterdam, Singapore and five US locations, unchanged since tracking began in late August.

Treat that as a market estimate, not a quote. A GPU configuration, a bigger bandwidth allotment or extra services will land well above it, and only the configuration screen returns a firm number. Reserved terms exist if you plan to keep the machine for a year or more, though the discount isn't published either.

## Four cores, two prices: what the extra $41 buys

|  | Vultr, 4 cores / 32 GB (tracked) | SpinServers Dallas, 4C/8T / 32 GB |
| --- | --- | --- |
| RAM | 32 GB | 32 GB DDR4 ECC |
| Local storage | 2×240 GB SSD | 1 TB M.2 NVMe |
| Monthly transfer | 5 TB | 30 TB on a 10 Gbps port |
| Billing | $0.164/hr or $120/mo, on-demand or reserved | $79/mo |
| Locations | 7 regions for this plan, 34 overall | Dallas and San Jose |

The hardware gap is storage and transfer, not compute. Four cores and 32 GB on both sides; one gives you 480 GB of local SSD and 5 TB of egress, the other 1 TB of NVMe and 30 TB. For a backup target, a media or game server pushing real traffic, or anything that stores data locally, that difference usually outweighs the price gap on its own.

What Vultr's premium does buy is real: hourly billing for clusters that live for days, presence in Amsterdam or Singapore that SpinServers' two US locations can't provide, and a GPU catalogue that SpinServers' current listings don't include. If your workload moves between regions or dies after a weekend, the monthly box is the wrong shape regardless of price.

👉 [Compare the full Dallas and San Jose inventory side by side](https://bit.ly/Spinservers)

## Which GPU tier matches your workload

Vultr's GPU bare metal spans four memory classes, and memory capacity decides more deployments than raw throughput does.

| GPU | Memory | Architecture |
| --- | --- | --- |
| NVIDIA L40S | 48 GB | Ada Lovelace |
| NVIDIA A100 (PCIe and SXM) | 80 GB | Ampere |
| NVIDIA H100 | 80 GB | Hopper |
| NVIDIA GH200 Superchip | 80 GB | Hopper |
| AMD Instinct MI300X | 192 GB | CDNA 3 |
| NVIDIA HGX B200 (per GPU) | 192 GB | Blackwell |
| AMD Instinct MI355X | 288 GB | CDNA 4 |

Rough selection logic: 48 GB (L40S) suits inference and rendering where the model and batch fit that budget. 80 GB covers training runs and larger inference on A100 or H100, with H100 adding FP8 throughput rated at 1979 TFLOPS. The 192 GB tier (MI300X, B200) is for models that need a single device to hold far more state, and B200 is rated at 4.5 PFLOPS FP8 per GPU. MI355X at 288 GB and 5.0 PFLOPS FP8 is the top of the range.

None of SpinServers' current Dallas or San Jose listings include a GPU. If a GPU is the reason you're shopping, the CPU configurations above aren't a cheaper substitute, and the decision sits entirely among GPU hosts.

## Four settings that are easy to get wrong at deploy time

These are documented behaviours on Vultr Bare Metal, and they're the kind of thing you discover after the machine is live, when fixing it costs a rebuild.

1. **SSH keys must be added before deployment.** Adding a key to an already-provisioned instance wipes the instance and loses its data. Load the key at creation or not at all.
2. **Cloud-Init under Terraform is fixed at creation.** The console, API and CLI can update user data on a running instance; Terraform cannot, and changing it requires recreating the server. Anything you'll want to tweak later should be scripted outside `user_data`.
3. **The disk option is a real fork.** RAID 1 mirrors disks in software; plain "No RAID" formats and mounts each disk individually; "No RAID (extra disks unformatted)" leaves additional disks untouched, and you format and mount them yourself. Choosing the third option by accident means finding unallocated disks an hour later.
4. **IPv6-only requires deselecting IPv4.** Public IPv4 is on by default; tick IPv6 and then clear IPv4 if that's the design. Private instances instead route out through a NAT gateway inside a VPC.

One more worth knowing: "Limited User Login" creates a `linuxuser` account with sudo as the default instead of root, which is a better starting point than handing out root credentials.

On the SpinServers side, provisioning is automated from the listing, so put the equivalent questions in the order notes: RAID layout, number of IPv4 addresses, and OS image.

👉 [See the 512 GB San Jose configuration and what it costs now](https://bit.ly/Spinservers)

## Who shouldn't buy bare metal here

- **A single small site or a dev box.** A VPS at a fraction of the price handles it, and you keep snapshots.
- **Workloads measured in hours.** Monthly billing makes a three-day cluster an expensive experiment. Hourly billing exists elsewhere for exactly this.
- **Multi-region requirements.** Two US locations won't cover a user base in Europe or Asia.
- **GPU work.** No GPU configurations appear in the current listings.

## Picking a configuration

Start from the workload, not the price:

- **One app server, a database under 50 GB, a game or voice server:** the 4C/8T 32 GB box at **$79/month**, Dallas or San Jose. San Jose's SJC-205 adds a second 10 Gbps port and a slightly higher turbo clock for the same money.
- **Virtualization host or a dozen containers with modest storage:** SJC-202 at **$115** (16C/32T, 64 GB, 1.6 TB SSD), or SJC-206 at **$155** if you want NVMe instead.
- **Memory-bound workloads (caches, analytics, dense VMs):** SJC-201 gives 256 GB and 2×1.6 TB SSD on hardware RAID with cache for **$225**, the cheapest route to a quarter terabyte of RAM in this lineup.
- **Raw core count and capacity:** the Dallas Platinum 8173M at **$390** (56C/112T, 512 GB, 4×1.92 TB) or SJC-204 at **$250** (32C/64T, 512 GB, 2×3.84 TB SSD with battery-backed RAID). SJC-204 costs $140 less and gives up 24 cores; if your workload scales with memory and disk rather than threads, take the San Jose box.

Before you order, confirm three things the listings don't spell out: renewal price after the first term, what 30 TB of monthly transfer costs to exceed, and how many IPv4 addresses ship with the specific configuration.

👉 [Order from the current bare metal listings](https://bit.ly/Spinservers)
