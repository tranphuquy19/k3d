# My K3D setup scripts

```bash
# K8s doesn't like Swapp
swapoff -a
apt install apt-transport-https curl wget -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
DEBIAN_FRONTEND=noninteractive apt install docker-ce docker-ce-cli containerd.io kubectl joe pbzip2 git openconnect -y
addgroup -a vagrant docker
systemctl enable docker
systemctl enable containerd
systemctl start docker
systemctl start containerd
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
wget -q -O - https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash
# let's pull the k3s container image once in the base image and reuse it - saves traffic and start up time every time a child image is started
k3d cluster create warmup-cluster --servers 1 --agents 0
k3d cluster delete warmup-cluster
apt-get clean
cat /dev/null > ~/.bash_history && history -c
dd if=/dev/zero of=/EMPTY bs=1M || echo "dd exit code $? is suppressed"
rm /EMPTY
sync
```

```bash
k3d cluster create --config ./k3d.yml
```
