# k3s-ha-cluster-setup
Setting up Kubernetes cluster using Rancher k3s with high availability with Ingress Nginx Controller

## Getting Started
In this setup, we'll be setting up 5 nodes cluster with 2 masters and 3 workers.

### Adding First Master Node
First we init a master node with `--init-cluster` option as well as setting a common token for all cluster to connect using `K3S_TOKEN` environment variable. `--init-cluster` will created an embedded ectd elimated to need to add external database. You can opt to use external database if required.

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=<COMMON_TOKEN> sh -s - server \
    --cluster-init \
    --disable traefik \
    --write-kubeconfig-mode 644 \
    --node-name master-01
```

### Adding Second Master Node

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=<COMMON_TOKEN> sh -s - server \
    --disable traefik \
    --write-kubeconfig-mode 644 \
    --node-name master-02 \
    --server https://<ip or hostname of server1>:6443
```

### Adding Worker Nodes

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=<COMMON_TOKEN> sh -s - agent --server https://<ip or hostname of server>:6443
```
We will run this command on all of 3 worker nodes.

### Installing Ingress Nginx Controller
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/cloud/deploy.yaml
```
## References
- [k3s Official Documentation](https://docs.k3s.io/)
- [Ingress Nginx Controller Documentation](https://kubernetes.github.io/ingress-nginx/)