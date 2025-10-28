# Homelab 

My goal is to create a GitOps-managed homelab server.

### General Ideas / Plans 

- [Release Please](https://github.com/googleapis/release-please) using [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- Proxmox 
  - Wipe and install Proxmox on Server PC!
- Talos? 
- Gateway API?
- ArgoCD + Kustomize?

Inspo: //github.com/theepicsaxguy/homelab/tree/main


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



### Phase X - "Production" 

- Host: old desktop computer (aka server-pc)
- OS: Proxmox 
  - TalOS nodes 
  - TrueNAS
  - ...


