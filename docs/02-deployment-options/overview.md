# Deployment Options

OpenSpace WebRTC can be deployed in multiple ways depending on scale,
hardware availability, and intended usage.

This section outlines the supported deployment models and helps you
choose the most appropriate option for your needs.

---

## Local Single-Machine Deployment

**Best for:**
- Development
- Testing
- Demos
- Running OpenSpace and the web browser on the same machine

**Characteristics:**
- Single host
- No cloud infrastructure required
- Simplest setup and lowest operational overhead

→ See `local-single-machine.md`

---

## Cloud Deployment (AWS EC2)

**Best for:**
- Public or remote access
- Multiple concurrent users
- GPU-backed rendering on remote hardware

**Characteristics:**
- GPU-enabled EC2 instances
- Additional network, security, and cost considerations
- Supports multiple OpenSpace instances per server
- Known limitations related to WebRTC video streaming in some setups

→ See `cloud-aws-ec2.md`
