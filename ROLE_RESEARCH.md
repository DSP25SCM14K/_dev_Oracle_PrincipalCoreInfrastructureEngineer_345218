# Oracle 345218 — company and role research

Research date: October 8, 2026. Research is separate from the portfolio narrative.

## Verified job

[Oracle requisition 345218](https://eeho.fa.us2.oraclecloud.com/hcmUI/CandidateExperience/en/sites/jobsearch/job/345218/) was read in Oracle’s candidate interface. Title: Principal Core Infrastructure Engineer. The description calls this an AI/ML Forward Deployed Infrastructure Engineer. Location: United States. Posted September 11, 2026, 10:28 AM as displayed. Regular individual contributor; 6–10+ years; English required; no security clearance required. US base hiring range displayed: $114,600–$234,600, with potential additional compensation.

Responsibilities combine strategic-customer support before and after sales, AI/ML infrastructure delivery, automation, performance and cost optimization, security, production troubleshooting, documentation, and liaison work with engineering/support. Required areas include Linux, containers, orchestration, scripting, networking, communication, and problem ownership. Preferred experience includes GPU/HPC, ML frameworks, programming, CI/CD, and monitoring. The posting emphasizes maintainability, mentoring, coordination, and candidate interviews.

The recommendations below are inferred from these requirements and the resume, not Oracle’s disclosed interview process.

## Oracle and its AI infrastructure offering

Oracle’s AI Infrastructure portfolio integrates bare-metal GPU compute, RDMA cluster networking, and high-performance storage for training, inference, and HPC. Current offerings extend beyond A100/H100 to newer GPU generations. For the portfolio, the candidate’s actual A100/H100 claims remain the relevant evidence. Product-scale maxima should not be interpreted as capacity available in a specific customer tenancy or a performance guarantee. [Oracle AI Infrastructure](https://www.oracle.com/ai-infrastructure/).

Oracle’s GPU architecture describes RoCE networking, GPU nodes, local NVMe, shared storage, and object storage alongside private connectivity and security boundaries. My interpretation is that this role’s customer value lies in making those pieces work together for a workload, then making the result reproducible and operable. A deployment proposal should explain data movement and cost drivers as clearly as compute sizing. [Oracle GPU reference architecture](https://docs.oracle.com/en/solutions/deploy-bare-metal-gpu-cluster-for-ai/index.html).

## Technical research and design implications

### GPU node readiness

OKE documentation requires an appropriate GPU shape and compatible image. Applications request GPU resources in their pod specifications. Supported shapes, images, service limits, quotas, and availability domains affect what can be deployed. GPU worker nodes belong to managed or self-managed configurations rather than virtual node pools. These details make a preflight inventory of capacity, permissions, images, and software compatibility valuable before committing a customer timeline. [Running GPU applications in OKE](https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengrunninggpunodes.htm).

### Scheduling and useful utilization

Kubernetes exposes GPUs through vendor device plugins as schedulable resources. GPU limits can supply the request by default; if requests are specified, they must match limits. Labels and node selection help workloads reach suitable devices. A generic GPU resource count alone does not communicate every property of the device or application. Useful utilization should be interpreted with application throughput, memory constraints, and latency. [Kubernetes GPU scheduling](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/).

Slurm manages GPUs as generic resources and supports GPU allocation and task binding. Its documentation explains typed resource requests and GPU accounting, with implementation-specific constraints. For distributed training and simulation work, a design discussion should make CPU/GPU affinity, job allocation, and resource accounting explicit rather than treating a scheduler as a black box. This is a useful comparison with Kubernetes, not evidence that one scheduler is universally preferable. [Slurm GRES scheduling](https://slurm.schedmd.com/gres.html).

### Scaling pods and capacity

Oracle separates node-pool autoscaling from pod autoscaling. Cluster Autoscaler changes available node capacity; HPA adjusts pod replicas; VPA adjusts requested resources. These mechanisms have different prerequisites and scopes. My inference is that GPU serving designs need a coordinated plan for pending pods, node provisioning delays, model loading, warm-up, and drain behavior. Adding replicas cannot fix an unavailable GPU node or a storage bottleneck. [OKE autoscaling](https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengautoscalingclusters.htm).

### Performance diagnosis and security

NCCL documentation organizes troubleshooting by communication and system boundaries. A practical investigation should isolate application compute, data preparation, local GPU communication, and inter-node transport before making broad tuning changes. The resume’s training-time and utilization improvements offer an opportunity to demonstrate controlled measurements and a clear causal explanation. [NCCL troubleshooting](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/troubleshooting.html).

Security should accompany the architecture from discovery onward: customer access boundaries, encrypted data, IAM/RBAC scope, network policy, patch ownership, and documented escalation. For this role, explaining a restriction or remediation clearly to customers is part of the technical work. These are inferred delivery practices; the portfolio’s specific SOC 2 and vulnerability results are candidate-supplied resume claims.

## Resume mapping and site decisions

The resume reports 45% lower training time, 38% lower GPU cost for 12 strategic customers, eight of ten POCs converted to production, provisioning reduced from three days to 45 minutes, and GPU utilization increasing from 52% to 87%. It also reports a 40% inference p99 reduction. The website preserves their original scope: it does not apply the 40% reduction to training or treat utilization as a throughput measurement.

All 15 experience bullets are included verbatim, including their measurement context. This avoids mixing figures from the separate 345217 resume. The site features AskMyStore and Voting App using the project descriptions in the 345218 resume and the supplied source/demo destinations. These are resume descriptions, not independent audits of repository implementations.

The visual background shows communicating GPU chips and storage. The main conceptual diagram connects GPU compute, RDMA, training data, and observability. The delivery diagram moves through discovery, provisioning, optimization, and operations. Rotating text expresses an interest in useful compute and customer delivery without explicitly naming an application target. Diagrams are conceptual rather than live monitoring.

All 23 unique supplied projects are retained. EXPOS is deduplicated. Only the four supplied technical demos are shown. The site includes GitHub, Scholar, LeetCode, paper, source notebook, and certificate links; no LinkedIn or footer.

## Inferred preparation themes

- Explain the baseline and changes behind the 45% training-time and 38% GPU-cost reductions.
- Walk through one POC acceptance plan, unresolved risk, production decision, and handoff.
- Explain GPU capacity readiness, image compatibility, automation, and rollback.
- Compare a Slurm training job with a Kubernetes inference service using real requirements.
- Reconstruct an incident with telemetry, customer communication, and documented remediation.
- Discuss secure access and network policy without obscuring customer workflow.

These themes are preparation suggestions, not confirmed interview questions.

