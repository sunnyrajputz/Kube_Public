# Follow the mentioned 3 steps:
## 1. Docker Installation<br>
## 2. Kubectl Installation<br>
## 3. Minikube Installation<br>

# ✅Docker Installation for Debian:
## Install using the apt repository

**1. Set up Docker's apt repository**

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

**2.Install the Docker packages.**
```bash
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**3.Verify that the installation is successful by running the hello-world image:**
```bash
 sudo docker run hello-world
```
**Reference:Ref: https://docs.docker.com/engine/install/debian/**


# ✅Kubectl Installation:
## **Step-by-Step: Install kubectl (no sudo)**
**If you’re a regular user (no root/sudo), here’s how to install it locally:**

**1. Download kubectl binary**
```bash
curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
**2. Make it executable**
```bash
chmod +x kubectl
```
**3. Move it to a folder in your PATH**
```bash
mkdir -p $HOME/bin
mv kubectl $HOME/bin/
```
**4. Add $HOME/bin to your PATH (if it isn't already)** <br>
Append this to your ~/.bashrc:
```bash
export PATH="$HOME/bin:$PATH"
```
**Then apply changes:**
```bash
source ~/.bashrc
```
**5.Test it**
```bash
kubectl version --client
```

# ✅Minikube Installation for Debian:
**1.To install the latest minikube stable release on x86-64 Linux using Debian package:**
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```
**2.Start your cluster**
```bash
minikube start
```
**3.Interact with your cluster**
```bash
kubectl get po -A
```
**Reference:https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fdebian+package**



