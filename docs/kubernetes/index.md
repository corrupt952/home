# Kubernetes cluster

## Architecture

The Kubernetes cluster at home is a cluster with multiple devices.    
See [Networks](/docs/networks/index.md) for the home network to which the cluster is connected.

```mermaid
graph TB

%% Definitions
subgraph k8s[Cluster]
    direction LR

    subgraph Distributed["Distributed control planes"]
        direction TB
        Plane1["Control plane1"]
        Plane2["Control plane2"]
        PlaneN["Control planeN"]
    end
    subgraph Nodes
        Node1
        Node2
        NodeN
    end
    
    Storage["Storage"]
end

%% Relations
Distributed --- Node1 & Node2 & NodeN
Plane1 ~~~ Plane2 ~~~ PlaneN
Node1 & Node2 & NodeN --- Storage
```

### Access from WAN

To access the various applications in the cluster from WAN, use the [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/).  
The Tunnel enables access from WAN without having to publish the server.  
It is also useful because authentication can be set up for access using Tunnel.

```mermaid
graph LR

%% Definitions
Edge[Cloudflare Edge]
subgraph k8s[Cluster]
  cloudflared
  ArgoCD[Argo CD]
  Dashboard[Kuberentes Dashboard]
  Others
end

%% Relations
WAN --> Edge
Edge -..->|Tunnel| cloudflared
cloudflared --> ArgoCD
cloudflared --> Dashboard
cloudflared --> Others
```

## Setup

[Set up the kubernetes cluster](/docs/kubernetes/setup.md)

## Upgrade

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
