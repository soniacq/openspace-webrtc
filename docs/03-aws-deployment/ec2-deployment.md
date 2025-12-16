# OpenSpace WebRTC on AWS EC2

This guide explains how to set up OpenSpace WebRTC on an AWS EC2 instance with Windows. It covers installing dependencies, configuring the environment, and running the application in both secure and non-secure modes. Following this tutorial ensures that your EC2 instance is correctly configured for GPU-accelerated OpenSpace rendering.

---

## A. EC2 Machine Requirements
To run OpenSpace WebRTC smoothly in the cloud, we recommend using a GPU-enabled instance.  
- **Instance Type:** G4dn.xlarge with Tesla T4 GPU  
- **AWS Minimum Requirements:** See [OpenSpace WebRTC Cloud Requirements](https://docs.google.com/document/d/1ou6ybwM5k764UGQGp1RoJcXfgEhIaRNMTvxDZx58NBo/edit?usp=sharing)

> **Note:** Using the right instance ensures that multiple OpenSpace instances can run concurrently without performance degradation.

---

## B. Connecting to the EC2 Machine Remotely

OpenSpace WebRTC requires a Windows environment. You can connect to your EC2 instance using Remote Desktop.

### Windows: Using Remote Desktop
1. Log in to the AWS EC2 Console as the root user.
2. Navigate to EC2 Dashboard → Select your Windows instance.
3. Retrieve the Administrator password:
   - Instance state → Start instance → Connect → RDP Client tab → Get Password
   - Upload `.pem` key to decrypt and get the password
4. Open Remote Desktop Connection (`Win + R` → `mstsc`).
5. Enter the public IP/DNS of your EC2 instance → Connect.
6. Login:
   - **Username:** Administrator  
   - **Password:** Use the decrypted password  
   - Accept certificate warning if prompted.

### Mac: Using Microsoft Remote Desktop
1. Install Microsoft Remote Desktop: [Mac App Store](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466)
2. Retrieve EC2 Administrator password as above.
3. Add a new PC in the Remote Desktop app:
   - Click `+` → Add PC → Enter public IP/DNS
   - Use "Administrator" and the decrypted password
4. Connect:
   - Double-click the PC entry
   - Accept certificate if prompted

> **Explanation:** Remote Desktop is required to manage the EC2 instance as if you were sitting in front of a physical Windows machine. This step is essential for installing software, configuring NVIDIA drivers, and running OpenSpace WebRTC.

---

## C. Enable Remote Procedure Call (RPC)
Windows uses RPC to allow different programs to communicate, including over the network.

1. Press `WIN + R` → `services.msc`
2. Locate `Remote Procedure Call (RPC)`
3. Ensure:
   - Status: Running
   - Startup Type: Automatic

> **Explanation:** Enabling RPC ensures that Remote Desktop and other inter-process communication works reliably.

---

## D. Install Required Software
Install the following tools to ensure the EC2 instance can build, run, and manage OpenSpace WebRTC:

- Git Bash: https://git-scm.com/downloads  
- CMake >= 3.31: https://cmake.org/download/  
- VS Code: https://code.visualstudio.com/  
- Visual Studio (C++ Make Tools module): https://visualstudio.microsoft.com/  
- Python (latest stable): https://www.python.org/downloads/  
  *(Ensure "Add Python to PATH" is checked)*  
- Node.js: https://nodejs.org/en/download/  
- PostgreSQL >=13: https://www.postgresql.org/download/  
- DBeaver (PostgreSQL GUI): https://dbeaver.io/download/  
- Qt 6.6.3 (MSVC build): https://www.qt.io/download-qt-installer

> **Explanation:** These dependencies are required to build, run, and maintain both the backend and frontend components of OpenSpace WebRTC on Windows.

---

## E. AWS CLI Setup (PowerShell)
AWS CLI allows you to manage your EC2 instance, S3 buckets, and other AWS services from PowerShell.

1. **Install AWS CLI**
```powershell
aws --version
# If not installed:
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```
Restart PowerShell.

2. **Configure AWS Credentials**
```powershell
aws configure
# Input IAM Access Key, Secret Key, default region (e.g., us-east-1), output json
```
Verify: `Get-Content ~/.aws/credentials`

> **Explanation:** AWS CLI is used later for managing NVIDIA driver installation and automation tasks.

---

## F. Install NVIDIA Drivers
Use AWS-provided NVIDIA drivers for your GPU. Do not download from NVIDIA website.
```powershell
$Bucket = "ec2-windows-nvidia-drivers"
$KeyPrefix = "latest"
$LocalPath = "$home\Desktop\NVIDIA"
$Objects = Get-S3Object -BucketName $Bucket -KeyPrefix $KeyPrefix -Region us-east-1
foreach ($Object in $Objects) {
    $LocalFileName = $Object.Key
    if ($LocalFileName -ne '' -and $Object.Size -ne 0) {
        $LocalFilePath = Join-Path $LocalPath $LocalFileName
        Copy-S3Object -BucketName $Bucket -Key $Object.Key -LocalFile $LocalFilePath -Region us-east-1
    }
}
```
Install manually after download.

> **Explanation:** Proper NVIDIA drivers are needed to run OpenGL and enable hardware-accelerated rendering for OpenSpace streaming.

---

## G. NVIDIA Control Panel Settings
Right-click desktop → Open NVIDIA Control Panel.

**Manage 3D Settings:**

- OpenGL Rendering GPU: NVIDIA RTX A10G

- Power Management Mode: Prefer Maximum Performance

> **Explanation:** Ensures OpenSpace uses the GPU correctly for streaming and graphics.

---

## H. AWS Instance Configuration
### H1. Permanent IP (Elastic IP)
- Allocate Elastic IP → Associate with your EC2 instance

- Update Security Groups to allow HTTP/HTTPS traffic

> **Explanation:** Ensures the instance keeps a consistent IP even after restarts, which is required for users to reliably access OpenSpace.

### H2. Inbound Rules for Public Access
- Configure Security Group to allow traffic (HTTP, HTTPS, SSH)

- Save changes

> **Explanation:** Without proper inbound rules, users cannot access OpenSpace from outside AWS.

---

## I. Setup Multiple OpenSpace Instances
- Set OPENSPACE_SYNC folder (e.g., `c:\Users\..\OpenSpace\sync`)

- Run `add_rendering_instance.py`

- More details: OpenSpace-Web-Backend

> **Explanation:** Multiple instances allow different users or sessions to run independently on the same EC2 machine.

---

## J. Running OpenSpace WebRTC
### 1. Start Supervisor (Web GUI + Signaling Server)
```bash
cd /c/webrtc/OpenSpace-Web-Backend
python supervisor.py
```

### 2. Start Backend Server
```bash
cd /c/webrtc/Backend-WebRTC/
npm start
```

### 3. Start Frontend UI Server
```bash
cd /c/webrtc/UI-WebRTC/
npm start
```

**Start Instance via UI:** Click Join Instance button in the browser.

> **Explanation:** Supervisor orchestrates all services; backend handles signaling and data, frontend provides the user interface.

---

## K. Secure vs. Non-Secure Mode
### Secure Mode (EC2 Access)
1. Update API in `api.js`:
```javascript
export default openspaceApi(Environment.wsAddress, Environment.wsPort, true);
```

2. Enable secure section in `.env`

3. Run proxy & Caddy servers:
```bash
cd /c/webrtc/OpenSpace-Web-Backend/secure_deployment/
python proxy.py

cd /c/Caddy/
./caddy_windows_amd64.exe run
```

4. Access via: `https://openspaceweb.com/frontend/#/streaming?id=0`

> ⚠️ **Note on Domain Name**
>
> The URL `https://openspaceweb.com/` shown in these instructions was a domain purchased and used by the OpenSpace WebRTC project at the time of writing.  
> When deploying your own EC2 instance, you should replace this with a domain that you own.  
> In AWS, you can easily register or associate a domain with your EC2 instance using **Route 53**.  
> After updating your domain, make sure to also update the corresponding `Environment.js` configuration in the Frontend repository.  
> For reference, see how this was done in this commit: [update domain in Environment.js](https://github.com/OpenSpace/OpenSpace-WebGuiFrontend/commit/ddde0bc2bc8a05e50500c7f873926399f3d06988)

### Non-Secure Mode (Local / Testing)
1. Update API in `api.js`:
```javascript
export default openspaceApi(Environment.wsAddress, Environment.wsPort, false);
```

2. Start instance:
```bash
cd /c/webrtc/OpenSpace-Web-Backend/testing
python start.py
```

3. Access via browser:

- Local: `http://localhost:4690/frontend/#/streaming?id=0`

- Other machines: add IP to `chrome://flags`

---

## Demo Video

To see OpenSpace WebRTC in action on an AWS EC2 instance, watch the following demo videos:

- **Non-secure connection** (inside the same machine, same network):  
  This demo shows the OpenSpace instance running on the EC2 Windows server and accessed locally within the same network.  
  [![Non-secure OpenSpace WebRTC demo](https://img.youtube.com/vi/lCfv9l-rLxg/1.jpg)](https://www.youtube.com/watch?v=lCfv9l-rLxg&start=0&end=304)

  <!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/lCfv9l-rLxg?start=126&end=304" title="Non-secure OpenSpace WebRTC demo" frameborder="0" allowfullscreen></iframe> -->

- **Secure connection** (Rendering server running on AWS instance, client accessing from outside the network):  
  This demo shows a secure deployment where the OpenSpace instance is hosted on EC2 and accessed remotely from a different network using HTTPS.  
  <!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/lCfv9l-rLxg?start=441" title="Secure OpenSpace WebRTC demo" frameborder="0" allowfullscreen></iframe> -->
  [![Secure OpenSpace WebRTC demo](https://img.youtube.com/vi/lCfv9l-rLxg/2.jpg)](https://www.youtube.com/watch?v=lCfv9l-rLxg&start=441)


---

## L. Additional Resources
- [OpenSpace-Web-Backend](https://github.com/OpenSpace/OpenSpace-Web-Backend)

- [Backend-WebRTC](https://github.com/OpenSpace/Backend-WebRTC)

- [UI-WebRTC](https://github.com/OpenSpace/UI-WebRTC)

---

## ⚠️ Known Issue (EC2 Deployment)
In some EC2 configurations, WebRTC video streaming may fail to render in the browser, even though user interaction and API communication remain functional. This is a known limitation and is left as future work.