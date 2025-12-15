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

- **Cloud (AWS EC2)**  
  OpenSpace runs on a GPU-enabled EC2 instance and streams to
  remote browsers over the internet.

## Documentation Structure

- `docs/00-introduction` — Overview, goals, and related repositories
- `docs/01-architecture` — System components and communication flow
- `docs/02-deployment-options` — Local and cloud deployment patterns
- `docs/03-aws-deployment` — AWS-specific requirements and scaling
- `docs/appendix` — Reference material and external links

## Related Repositories

This repository does **not** duplicate the implementation or build
instructions of the OpenSpace WebRTC components. Instead, it
provides system-level documentation and deployment guidance.

Core components:
- https://github.com/OpenSpace/OpenSpace-Web-Backend
- https://github.com/OpenSpace/Backend-WebRTC
- https://github.com/OpenSpace/UI-WebRTC
