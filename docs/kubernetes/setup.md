# Set up a kubernetes cluster

## 1. Install Ubuntu to devices

Set up using CD, DVD, or USB installation media.  
Set up with only minimal configuration, and set up SSH public keys to be obtained from GitHub.

https://ubuntu.com/tutorials/install-ubuntu-server

> Be sure to generate and use a password for HomeLab management

## 2. Configure static IP

I want to fix the IP address before running Ansible, so set up `netplan`.

1. Set up `/etc/netplan/99-config.yaml`.

    ```yaml
    # /etc/netplan/99-config.yaml
    network:
      version: 2
      ethernets:
        eno1: # Configure network adapter name
          dhcp4: yes
          dhcp6: no
          routes:
          - to: default
            via: 192.168.x.1 # Configure Gateway
          nameservers:
            addresses: [1.1.1.1, 1.0.0.1] # Configure Nameservers
          addresses: # Configure static IP
          - 192.168.x.y/24
          - 192.168.x.z/24
    ```

1. Apply a netplan configuration

    ```sh
    sudo netplan apply
    ```

## 3. Run playbooks

Run `ansible-playbook` to set up each device.

1. Move to `ansible` directory

    ```sh
    cd ansible
    ```

1. Make `.vault-password` with a written password for ansible-vault

    ```sh
    echo -n "$VAULT_PASSWORD" > .vault-password
    ```

1. Decrypt `hosts.enc`

    ```sh
    ansible-vault decrypt --output hosts hosts.enc
    ```

1. Install plugins

    ```sh
    ansible-galaxy collection install yamaha_network.rtx
    ```

1. Dry-run ansible-playbook

    ```sh
    ansible-playbook site.yaml -i hosts --check
    ```

    Specify target
    ```sh
    ansible-playbook site.yaml -i hosts --check -l 192.168.x.y
    ```

1. Run ansible-playbook

    ```sh
    ansible-playbook site.yaml -i hosts
    ```
    
    Specify target
    ```sh
    ansible-playbook site.yaml -i hosts -l 192.168.x.y
    ```

## 4. Join worker nodes

1. Generate join command on control-plane

    ```sh
    kubeadm token create --print-join-command
    ```

1. Workers join a cluster using a command generated by `kubeadm token create`

## 5. Install Calico

1. Install Calico on cluster via root user

    ```sh
    kubectl apply -f https://projectcalico.docs.tigera.io/manifests/calico.yaml
    ```

## 6. Install Argo CD

1. Add repository for Argo projects

    ```sh
    helm repo add argo https://argoproj.github.io/argo-helm
    ```

1. Install Argo CD via Helm

    ```sh
    helm install argocd argo/argo-cd --create-namespace --namespace argocd
    ```

## 7. Create an Application

1. You need access to the Argo CD so you can access it using Port Forwarding

    ```sh
    kubectl -n argocd port-forward deploy/argocd-server 8080:8080
    ```

1. Log in to Argo CD with `argocd` CLI

    ```sh
    argocd login \
        --insecure \
        --grpc-web \
        --username admin \
        --password "$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)" \
        localhost:8080
    ```

1. Create an application(`argocd-config`)

    ```sh
    argocd app create argocd-config \
        --insecure \
        --grpc-web \
        --repo https://github.com/corrupt952/home.git \
        --path manifests/argocd-config/base \
        --dest-namespace argocd \
        --dest-server https://kubernetes.default.svc \
        --sync-policy automated \
        --auto-prune \
        --revision main
    ```

1. Access [localhost:8080](http://localhost:8080) and synchronize the `argocd-config` application 
