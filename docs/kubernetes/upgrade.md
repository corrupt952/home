# Upgrade a kubernetes cluster

Upgrading a Kubernetes cluster is done in the order of Control-Plane -> Worker-Node.

## 1. Drain the node

Drain the node to remove all pods from the node.

```sh
kubectl drain --ignore-daemonsets --delete-emptydir-data $HOSTNAME
```

## 2. Update keyrings and repositories

```sh
VERSION=v1.29
curl -fsSL https://pkgs.k8s.io/core:/stable:/$VERSION/deb/Release.key | gpg --dearmor | sudo tee /usr/share/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://pkgs.k8s.io/core:/stable:/$VERSION/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

After that, update the package list.

```sh
sudo apt update
```

## 3. Upgrade kubeadm(only Control-Plane)

Upgrade `kubeadm` on the control-plane node.

```sh
VERSION=1.29.6-1.1
sudo apt-cache madison kubeadm
sudo apt-mark unhold kubeadm && sudo apt-get update && sudo apt-get install -y kubeadm=$VERSION && sudo apt-mark hold kubeadm
```

After that, upgrade the control-plane.

```sh
VERSION=v1.29.6
sudo kubeadm upgrade plan
sudo kubeadm upgrade apply $VERSION
sudo kubeadm upgrade node
```

## 4. Upgrade kubelet and kubectl

Upgrade `kubelet` and `kubectl` on the node.

```sh
VERSION=1.29.6-1.1
sudo apt-mark unhold kubelet kubectl && sudo apt-get update && sudo apt-get install -y kubelet=$VERSION kubectl=$VERSION && sudo apt-mark hold kubelet kubectl
```

## 5. Reload systemd

```sh
sudo systemctl daemon-reload
```

## 6. Reboot

```sh
sudo reboot
```

## 7. Uncordon the node

```sh
kubectl uncordon $HOSTNAME
```
