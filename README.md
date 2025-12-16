# OpenSpace WebRTC

OpenSpace WebRTC enables real-time streaming of OpenSpace-rendered
visualizations to a standard web browser using GStreamer and WebRTC.

**Goal:** Stream OpenSpace rendering from any computer into a web browser,
without requiring OpenSpace to run on the client machine.

## What is OpenSpace?
[OpenSpace](https://www.openspaceproject.com/) is an open-source
astrophysical data visualization platform designed for interactive,
high-performance rendering of large-scale space datasets.

## What does OpenSpace WebRTC add?
OpenSpace WebRTC allows OpenSpace to run on a rendering machine
(local or cloud-based) while streaming the rendered frames to
remote clients through a web browser.

This enables:
- Thin-client access (browser only)
- Remote and cloud-based deployments
- Scalable, multi-user scenarios

## Deployment Options
OpenSpace WebRTC can be deployed in multiple ways:

- **Local (Single Machine)**  
  OpenSpace and the WebRTC stack run on the same computer and are
  accessed via a web browser (local or LAN).  
  [See Local Deployment Instructions](docs/02-deployment-options/local-single-machine.md)

- **Cloud (AWS EC2)**  
  OpenSpace runs on a GPU-enabled EC2 instance and streams to
  remote browsers over the internet.  
  [See AWS Deployment Overview](docs/03-aws-deployment/overview.md)  

  Key AWS Docs:  
  - [Cost & Scaling](docs/03-aws-deployment/cost-and-scaling.md)  
  - [Instance Requirements](docs/03-aws-deployment/instance-requirements.md)  
  - [Full EC2 Deployment Tutorial](docs/03-aws-deployment/ec2-deployment.md)  

## System Architecture
Learn about OpenSpace WebRTC architecture and communication flow:  
[System Architecture](docs/01-architecture/system-architecture.md)  
![Architecture Diagram](docs/01-architecture/images/openspace_webrtc-architecture_unsecure.png)  

## Related Repositories

This repository does **not** duplicate the implementation or build
instructions of the OpenSpace WebRTC components. Instead, it
provides system-level documentation and deployment guidance.

Core components:
- [OpenSpace-Web-Backend](https://github.com/OpenSpace/OpenSpace-Web-Backend)
- [Backend-WebRTC](https://github.com/OpenSpace/Backend-WebRTC)
- [UI-WebRTC](https://github.com/OpenSpace/UI-WebRTC)

## Appendix
Reference material, external links, and additional resources:  
[Appendix](docs/appendix)
