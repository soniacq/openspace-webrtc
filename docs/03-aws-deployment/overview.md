# AWS EC2 Deployment Overview

OpenSpace WebRTC can be deployed on Amazon EC2 to provide GPU-backed remote access for multiple concurrent users. This document provides a high-level overview and points to more detailed guidance.

---

## Key Considerations

- **Secure Connections Required:** All EC2 deployments must use secure WebSocket (wss://) and HTTPS connections to allow access from other machines and networks.  
- **Multiple Users:** Different users can access different OpenSpace instances concurrently. If multiple users attempt to access the same instance, the last user will gain control, and previous users may lose access.  
- **Cost Implications:** Using EC2 instances with GPU support and remote access incurs AWS charges. Ensure that you monitor usage to manage costs.  

---

## Documentation Structure

For detailed information on AWS deployment, consult the following files:

| File | Description |
|------|-------------|
| [`cost-and-scaling.md`](cost-and-scaling.md) | Guidance on EC2 instance types, GPU options, and cost considerations. |
| [`instance-requirements.md`](instance-requirements.md) | Hardware and software requirements for EC2 instances running OpenSpace WebRTC. |
| [`ec2-deployment.md`](ec2-deployment.md) | Step-by-step tutorial: setting up an EC2 instance, installing dependencies, configuring OpenSpace WebRTC, and securing endpoints. |

---

This overview provides context and points to the detailed resources needed to deploy OpenSpace WebRTC successfully on AWS EC2.

 
⚠️ **Known Issue (EC2 Deployment):** In some EC2 configurations, WebRTC video streaming may fail to render in the browser, even though user interactions and API communication remain functional. This is a known limitation and is left as future work.
