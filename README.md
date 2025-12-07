# Homelab 

My goal is to create a GitOps-managed homelab server.

### General Ideas

- [Release Please](https://github.com/googleapis/release-please) using [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- Proxmox 
  - Wipe and install Proxmox on Server PC!
- Talos? 
- Gateway API?
- ArgoCD + Kustomize?
- [KubeChecks](https://github.com/zapier/kubechecks)?
- [Kargo](https://kargo.io/)?
- ~~[Homarr](https://homarr.dev/)~~ [Homepage](https://github.com/gethomepage/homepage)
  - 11/1/25: decided to use Homepage. 
- Promethus & Graphana ([Beszel](https://github.com/henrygd/beszel) for short term)
- [Shoutrrr](https://github.com/containrrr/shoutrrr) for notifications? 
- Longhorn?

Inspo: https://github.com/theepicsaxguy/homelab/tree/main


### Phase 1 - Learning Phase 

Use `minikube` locally with the end-goal of K8s on the server PC. 

### minikube 
```
minikube start --driver=docker
kubectl get nodes
```

Easy reset:  (`minikube delete && minikube start`)

Install ArgoCD in minikube:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

access it: 
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
- https://localhost:8080

get default admin password:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```


When adding a new service, for now, we'll have to add the hostname to /etc/hosts/, e.g.,
```
echo "192.168.49.2  springboot-demo.local" | sudo tee -a /etc/hosts
```

### Beszel Monitoring

Access Beszel UI:
```
kubectl port-forward svc/beszel-hub -n monitoring 8090:8090
```
- http://localhost:8090

**Initial Setup:**
1. Access the Beszel hub UI (first visit creates admin account)
2. Create a new system to get the public KEY
3. Update the agent DaemonSet with the KEY and HUB_URL:
   - Edit `k8s/applications/monitoring/beszel/agent-daemonset.yaml`
   - Uncomment and set the `KEY` and `HUB_URL` environment variables
   - The hub URL inside the cluster is: `http://beszel-hub.monitoring.svc.cluster.local:8090`
4. Commit and push - ArgoCD will sync the updated agent configuration



### Phase X - "Production" 

- Host: old desktop computer (aka server-pc)
- OS: Proxmox 
  - TalOS nodes 
  - TrueNAS
  - ...


