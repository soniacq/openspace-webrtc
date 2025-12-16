# Local Single-Machine Deployment
This deployment runs **all OpenSpace WebRTC components on the same machine**
in **non-secure mode (HTTP / WS)**.
It is intended for:
- Development
- Testing
- Demos
- Running OpenSpace and the browser on the same machine
Secure mode (HTTPS / WSS) is **not used in local deployments** and is only
required when accessing OpenSpace WebRTC from **another network**, such as
in an AWS EC2 deployment.
---
## Required Repositories
You must clone and set up the following repositories locally:
- OpenSpace Web Backend  
  https://github.com/OpenSpace/OpenSpace-Web-Backend
- Backend WebRTC Server  
  https://github.com/OpenSpace/Backend-WebRTC
- UI WebRTC (Frontend)  
  https://github.com/OpenSpace/UI-WebRTC

Refer to each repository for detailed build instructions and dependencies.

---
## Overview (Local, Non-Secure)
In a local single-machine setup:
- All components run on the same host
- Communication uses **HTTP and WebSocket (WS)**
- No TLS, reverse proxy, or certificates are required
- OpenSpace instances are typically started **from the web UI**
---
## Step 1: Disable Secure WebSocket in the Frontend
Local deployment **requires disabling secure WebSockets**.
Edit:
`OpenSpace-Web-Backend/OpenSpace-WebGuiFrontend/src/api/api.js`
```cpp
export default openspaceApi(Environment.wsAddress, Environment.wsPort, true);
```
Change to:
```js
export default openspaceApi(Environment.wsAddress, Environment.wsPort, false);
```
This must be done before starting any services.
## Step 2: Run the Supervisor
The supervisor automatically starts:
- Web GUI on port 4690
- Signaling server on port 8443
```bash
# Navigate to your OpenSpace-Web-Backend directory
cd /OpenSpace-Web-Backend
python supervisor.py
```
## Step 3: Start the Backend WebRTC Server
```bash
# Navigate to your Backend-WebRTC directory
cd /Backend-WebRTC
npm start
```
## Step 4: Start the Frontend UI Server
```bash
# Navigate to your UI-WebRTC directory
cd /UI-WebRTC
npm start
```
Once started, the UI will load in the browser.
## Step 5: Start an OpenSpace Instance via the UI (Recommended)
1. Open the browser on the same machine
2. Navigate to:
```bash
http://localhost:4690/frontend/
```
3. Click "Join instance"
This will:
- Request a new OpenSpace instance
- Start OpenSpace via the supervisor
- Automatically connect the Web GUI
- Begin streaming when initialization completes
This is the normal and recommended workflow for local deployment.
## Optional: Start OpenSpace Manually (Advanced / Debugging)
If needed, an OpenSpace instance can be started manually:
```bash
cd /c/webrtc/OpenSpace-Web-Backend/testing
python start.py
```
Then access:
```bash
http://localhost:4690/frontend/#/streaming?id=0
```
## Access from Another Machine (Non-Secure)
To access the local deployment from a different machine:
```bash
http://<ip_address>:4690/frontend/#/streaming?id=0
```
⚠️ The IP address must be added to chrome://flags to allow non-secure
content.
This approach is intended for testing only.
## Important Notes
- Local deployment runs entirely in non-secure mode
- No authentication or encryption is provided
- Not suitable for public or production use
- Secure access is addressed in cloud deployments only

For secure, cross-network access, see:
→ cloud-aws-ec2.md