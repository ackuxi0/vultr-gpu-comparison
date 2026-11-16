# Vultr GPU: Which Card Do You Need, What Does It Cost, and Is a Cluster Worth It? (A100, HGX B200, MI355X and 8 More Compared)

Vultr lists the NVIDIA A100 PCIe from **$1.290 per hour** and the NVIDIA HGX B200 from **$3.500 per hour**. Those are the only two hourly GPU rates printed on the product pages reviewed for this article, and they appear here with the same decimals Vultr uses.

The A100 page shows the figure two ways, "$1.29/ Per hour" and "$1.290 / hour", which is the same number spelled twice. The B200 page only ever writes "$3.500 / Per hour", with nothing to disambiguate it, so I have not converted it to $3.50 or $3,500 on Vultr's behalf. Confirm that figure in the console before you plan a budget around it.

The other nine cards in the lineup are described by workload and specification, not by price. So the practical order of operations is: match the card to the job, then price it at deploy time.

## The short version

> **Single card, single job:** A100 80GB PCIe, from $1.290/hour, available as a virtualized instance or bare metal. Fine-tuning a 7B–13B model, batch inference, data processing, a simulation run you'll destroy tonight.
>
> **Eight GPUs over NVLink:** HGX B200, from $3.500/hour as listed. Large-scale training, LLM inference at volume, HPC. This is a node, not a card you rent casually.
>
> 👉 [See the A100 PCIe listing and current price](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J)

## Every GPU Vultr currently lists

Vultr's own documentation (updated 4 November 2026) names 11 Cloud GPU variants, running from an A16 up to AMD's MI355X and NVIDIA's Blackwell-based HGX B200.

| GPU | What Vultr positions it for | Verified spec | Published hourly price | Page |
| --- | --- | --- | --- | --- |
| AMD Instinct MI355X | Very large model training and inference, large-scale simulation | CDNA 4, 288 GB HBM3e per GPU, FP8/FP4 | Not published on pages reviewed | [MI355X](https://www.vultr.com/products/cloud-gpu/amd-mi355x/?ref=9738262-9J) |
| NVIDIA HGX B200 | Generative AI, LLM training, HPC | 8 Blackwell GPUs, up to 192 GB HBM3e each | **Starting at $3.500 / hour** | [HGX B200](https://www.vultr.com/products/cloud-gpu/nvidia-b200?ref=9738262-9J) |
| AMD Instinct MI325X | AI training and inference where memory capacity is the constraint | Large-capacity GPU memory | Not published on pages reviewed | [Vultr GPU lineup](https://bit.ly/VuLtr) |
| AMD Instinct MI300X | Training and inference, scalable data processing, scientific simulation | High memory bandwidth | Not published on pages reviewed | [MI300X](https://www.vultr.com/products/cloud-gpu/amd-mi300x/?ref=9738262-9J) |
| NVIDIA GH200 Superchip | Large AI deployments, simulation, analytics | Combines multiple GPU components | Not published on pages reviewed | [GH200](https://www.vultr.com/products/cloud-gpu/nvidia-gh200/?ref=9738262-9J) |
| NVIDIA HGX H100 | LLM training and inference, compute-heavy research | Tensor Cores, SXM | Not published on pages reviewed | [H100 / H200](https://www.vultr.com/products/cloud-gpu/nvidia-h100-h200/?ref=9738262-9J) |
| NVIDIA L40S | Graphics, AI inference, real-time processing, 3D visualization, video | — | Not published on pages reviewed | [L40S](https://www.vultr.com/products/cloud-gpu/nvidia-l40s/?ref=9738262-9J) |
| NVIDIA HGX A100 (SXM) | Training large neural networks | 80 GB HBM2e, 2,039 GB/s, 400 W | Not published separately | [A100](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J) |
| NVIDIA A100 PCIe | AI training, deep learning, HPC | 80 GB HBM2e, 1,935 GB/s, 300 W | **Starting at $1.290 / hour** | [A100](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J) |
| NVIDIA A40 | 3D rendering, virtual production, visualization-heavy simulation | High memory capacity | Not published on pages reviewed | [A40](https://www.vultr.com/products/cloud-gpu/nvidia-a40/?ref=9738262-9J) |
| NVIDIA A16 | Listed as a Cloud GPU variant | — | Not published on pages reviewed | [Vultr GPU lineup](https://bit.ly/VuLtr) |

Eight of those eleven rows say "not published" for a reason. Vultr's per-model product pages explain what each card is for and who should rent it, but the hourly rate only appears on the A100 and B200 pages. If a comparison site hands you an L40S or H100 number without a date and a source, treat it as a starting point to verify rather than a quote.

## A100 80GB: the card most people should start with

The A100 is the only GPU where Vultr publishes both a rate and a full specification table, and the spec sheet is the interesting part.

Both variants carry **80 GB of HBM2e**. The PCIe card moves data at **1,935 GB/s**, the SXM card at **2,039 GB/s**. Compute is identical across the two: 9.7 TFLOPS FP64, 19.5 TFLOPS FP32, 156 TFLOPS TF32 (312 with sparsity), 312 TFLOPS FP16 Tensor Core (624 with sparsity), and the same for INT8.

A 5% bandwidth gap does not decide anything on its own. The power ceiling and the server options do.

|  | A100 80GB PCIe | A100 80GB SXM |
| --- | --- | --- |
| Memory | 80 GB HBM2e | 80 GB HBM2e |
| Memory bandwidth | 1,935 GB/s | 2,039 GB/s |
| Max TDP | 300 W | 400 W |
| MIG | Up to 7 MIGs @ 10 GB | Up to 7 MIGs @ 10 GB |
| Interconnect | NVLink bridge for 2 GPUs, 600 GB/s; PCIe Gen4 64 GB/s | NVLink 600 GB/s; PCIe Gen4 64 GB/s |
| Server options | NVIDIA-Certified systems with 1–8 GPUs | HGX A100 with 4, 8, or 16 GPUs; DGX A100 with 8 |

Read the last row as the real distinction. PCIe is documented for single-GPU and small multi-GPU systems, so it's the shape you want when you're renting one card or a pair. SXM only earns its extra 100 W when you're stringing four or more GPUs together inside an HGX chassis, where the SXM form factor's power delivery and thermals are what Vultr cites as the reason to pick it.

**MIG is the feature that changes how you use one card.** A single A100 can be partitioned into up to seven isolated instances, each with dedicated memory and compute, at roughly 10 GB per slice. Vultr supports MIG, which means you can serve several small models side by side, or split one card across tenants, without the workloads contending for the same memory. It does nothing for a 70B model that needs the whole 80 GB. For a team running three or four 7B-class endpoints, seven 10 GB slices can be cheaper than seven separate GPUs.

Vultr also sells the A100 two ways: as a virtualized instance or as bare metal. Virtualized is the default for most people. Bare metal makes sense when you want the whole host, no hypervisor overhead, and predictable NUMA behaviour for a long training run.

👉 [Deploy an A100 80GB PCIe instance](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J)

## HGX B200: an eight-GPU node, with a preliminary spec sheet

The B200 is a different purchase category. Vultr's spec table lists **8 Blackwell GPUs** in the unit, **up to 1.5 TB of fast memory** across them, **up to 64 TB/s of aggregate memory bandwidth** and **14.4 TB/s of aggregate NVLink bandwidth**, with **up to 192 GB of HBM3e per GPU**. Single-GPU interconnect is fifth-generation NVLink at **1.8 TB/s**. Tensor throughput is quoted at **144 petaFLOPS FP4** and 72 petaFLOPS FP8, both with sparsity.

Vultr footnotes that table "Preliminary specifications. May be subject to change." Take the footnote seriously. If a number in your capacity plan has to be exact, get it reconfirmed before you commit a long job to the node.

The performance claims on the B200 page come with a stated baseline, which makes them usable: against NVIDIA Hopper GPUs, Vultr quotes GPT-MoE-1.8T training 3x faster, the same model's inference 15x faster, and HPC plus data analytics 2x faster. The Blackwell Decompression Engine handles LZ4, Snappy and Deflate, at 6x CPU speed and 2x H100 speed on the same page. The separate Vultr Discover page for the B200 also advertises "3x lower cost" and "12x improved energy efficiency" with no baseline or method attached. Those two are marketing lines, not planning inputs.

Two practical consequences follow from the 8-GPU configuration. First, B200 capacity on Vultr is organised as clusters connected by NVIDIA InfiniBand, not as single cards you spin up for an hour. Second, you should ask whether the $3.500 figure is a per-GPU rate or a per-node rate before you compare it to anything. An eight-GPU node at $3.500 per GPU-hour and an eight-GPU node at $3.500 per node-hour are very different purchases, and the product page doesn't say which it is.

👉 [Check the HGX B200 listing and cluster options](https://www.vultr.com/products/cloud-gpu/nvidia-b200?ref=9738262-9J)

## How you buy and how you scale

Everything GPU on Vultr is self-service. There is no sales call, no capacity request form, and no reservation queue. Vultr's wording on both the A100 and B200 pages is "no reservations or requests are required", which applies to Vultr Clusters on both Cloud GPU and Bare Metal.

Clusters are provisioned from the Vultr Console or the API, and scale up or down as the workload changes. You can attach high-performance storage, watch cluster metrics, and schedule work with **Slurm or Kubernetes** rather than wiring a scheduler yourself. For a small team, that's usually the deciding factor between renting bare GPUs somewhere and renting a cluster here: the orchestration layer is already in the product.

Placement matters more than people expect. Vultr runs **34 cloud data center regions across six continents**, so you can put the GPU near the data rather than moving terabytes to the GPU. Vultr also states that its platform, products and services are covered by frameworks including ISO and SOC 2+, which is the sort of thing that matters when someone else's compliance team reviews your architecture.

👉 [Open a GPU cluster from the Vultr console](https://bit.ly/VuLtr)

## When Vultr GPU is the wrong choice

The A100 and B200 get the attention, but most workloads that get quoted an A100 don't need one.

**Rendering, virtual production, video processing, VDI.** The L40S is positioned for graphics, AI inference and real-time processing, including 3D visualization and video pipelines. The A40 covers 3D rendering, virtual production and visualization-heavy simulation. Neither is priced on the pages reviewed, so get the rate before you assume the bigger card is the safer pick.

**A model too large for 80 GB.** AMD's MI355X carries **288 GB of HBM3e per GPU** with FP8 and FP4 support, built for very large model training and inference plus large-scale scientific computing. The MI325X targets workloads where memory capacity is the binding constraint, and the MI300X trades on memory bandwidth for training, inference and simulation. If your model doesn't fit on an A100, adding a second A100 is often worse than moving to one card with more memory.

**Sharing one physical GPU across many VMs.** Vultr's docs distinguish vGPU (one physical GPU shared among multiple VMs, aimed at VDI, graphics apps and shared AI setups) from passthrough (the whole GPU assigned to a single VM for maximum performance and compatibility). Decide which model you want before you pick a card, because they lead to different instance types.

**A five-minute experiment on a tight budget.** The B200 is an eight-GPU node. If your job finishes in four minutes, you're paying for a lot of hardware you won't touch.

## FAQ

**Can I rent a single GPU?**
Yes for the A100. Vultr documents the A100 PCIe for NVIDIA-Certified systems with 1 to 8 GPUs, and offers it virtualized or as bare metal. The HGX B200 is specified as an 8-GPU unit, so it's a node purchase.

**What is MIG and do I need it?**
Multi-Instance GPU partitions one A100 into up to seven isolated instances at roughly 10 GB each, each with its own memory and compute. Vultr supports it. Use it to serve several small models or separate tenants on one card; skip it if your model needs the full 80 GB.

**Does it run PyTorch and TensorFlow?**
Yes. Vultr's A100 documentation lists CUDA, TensorFlow, PyTorch and RAPIDS support, on Linux distributions including Ubuntu, CentOS and Debian.

**How fast is deployment?**
Vultr's Discover page for the B200 claims deployment in under 20 seconds with drivers and software pre-configured. That's the vendor's claim on that page, not an independent measurement.

**Do I need to reserve capacity in advance?**
No. Vultr Clusters are on-demand and self-service through the console or API, with no reservation or request step.

**Where are the GPUs?**
Across 34 cloud data center regions on six continents, per Vultr's B200 page.

**Why can't I find a price for the L40S or H100?**
Because the product pages reviewed here don't publish one. Vultr only prints hourly rates on the A100 and B200 pages. Get the number from the console before you build a budget on a third-party figure.

## Before you deploy

Match the card to the job first, then price it:

- One model that fits in 80 GB, one job at a time → A100 80GB PCIe.
- Four or more GPUs training together in one chassis → A100 SXM in an HGX configuration.
- Several small models or multiple tenants on one card → A100 with MIG enabled.
- Frontier-scale training or high-volume LLM inference → HGX B200, and confirm the spec sheet is no longer marked preliminary.
- Rendering, video or virtual production → L40S or A40, once you have the rate.
- A model that won't fit in 80 GB → MI355X or MI300X.

Two things to confirm before you launch anything long-running: whether the B200's $3.500 is per GPU or per node, and what the billing increment is, since Vultr's GPU pages don't state a minimum billing period. Both answers change the maths more than the headline rate does.

The sign-up page linked below advertises a $300 code credit. The eligibility rules and expiry aren't published on the GPU pages, so read them at checkout rather than assuming the credit applies to a GPU instance.

👉 [Start from Vultr's GPU page and check current pricing](https://bit.ly/VuLtr)
