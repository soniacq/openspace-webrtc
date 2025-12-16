# EC2 Cost and Scaling Considerations for OpenSpace WebRTC

This document provides guidance for selecting AWS EC2 instances to run OpenSpace WebRTC efficiently. It covers minimum requirements, recommended instance types, scaling strategies, and estimated costs.

---

## Minimum Requirements

| Component | Minimum Requirement |
|-----------|------------------|
| CPU | Intel i5 equivalent or better |
| GPU | NVIDIA GTX 1060 or comparable |
| RAM | 8 GB |
| VRAM | 4 GB |
| Software Size | ~10 GB (including sync folder) |
| OS | Windows |
| Instances per Machine | Multiple OpenSpace instances supported |

---

## Recommended EC2 Instances

| Instance Type | GPU | vCPUs | RAM | VRAM | Max OS Instances | Hourly Price | Notes |
|---------------|-----|-------|-----|------|-----------------|--------------|-------|
| G4dn.xlarge | NVIDIA T4 (16GB) | 4 | 16 GB | 16 GB | 1-2 | ~$0.76 | Best for MVP |
| G3s.xlarge | NVIDIA M60 (8GB) | 4 | 30.5 GB | 8 GB | 2-3 | ~$0.97 | Older but supports more instances |
| G5.xlarge | NVIDIA A10G (24GB) | 4 | 16 GB | 24 GB | 3-4 | ~$1.20 | Overkill but future-proof |
| G5.2xlarge | NVIDIA A10G (24GB) | 8 | 32 GB | 24 GB | 5-6 | ~$2.40 | Best for production |

**Legend:**
- **MVP**: Minimal Viable Product (2 developers, limited instances).  
- **G3**: Older GPU generation for graphics-heavy tasks.  
- **G4**: NVIDIA T4 GPUs, balanced performance.  
- **G5**: Latest NVIDIA A10G GPUs, high performance for advanced graphics.  

---

## Monthly Cost Estimation

Assuming development/testing for ~6 hours/day:

| Instance Type | Monthly Cost (24/7) | Monthly Cost (~6 hr/day) |
|---------------|-------------------|-------------------------|
| G4dn.xlarge (1-2 instances) | ~$547.20 | ~$136.80 |
| G3s.xlarge (2-3 instances) | ~$700.80 | ~$175.20 |
| G5.xlarge (3-4 instances) | ~$864.00 | ~$216.00 |
| G5.2xlarge (5-6 instances) | ~$1,728.00 | ~$432.00 |

> For pricing details, see [AWS EC2 On-Demand Pricing](https://aws.amazon.com/ec2/pricing/on-demand/)

---

## Recommendation Based on Use Case

- **MVP (2 Developers, Limited Instances)** → **G4dn.xlarge**  
  Runs 1-2 OpenSpace instances smoothly, best price/performance balance.

- **Scaling Beyond MVP (More Users, Heavier Load)** → **G3s.xlarge** or **G5.xlarge**  
  - G3s.xlarge: Supports 2-3 instances at moderate cost.  
  - G5.xlarge: Handles 3-4 instances, better for larger tests.

- **Future Production Deployment (Multiple Concurrent Users)** → **G5.2xlarge**  
  Handles 5-6 instances, future-proof with 8 vCPUs & 32GB RAM.  

---

## Scaling Strategy

- Launch additional EC2 instances when CPU usage exceeds ~75%.  
- Scale down instances when CPU usage drops below ~75%.  
- This ensures OpenSpace is available continuously while optimizing cost.  
- Scaling can be managed via AWS Auto Scaling Groups (ASGs).

---

**Developer Suggestion:**  
For initial development, use **G4dn.xlarge**. Upgrade to **G5.xlarge** or **G5.2xlarge** as needed for larger workloads or production use.
