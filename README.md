# KubeZT - Zero Trust Kubernetes
![KubeZT Logo](https://kubezt-public.s3.us-gov-east-1.amazonaws.com/kubezt_logo_black.png)

<p align="center">
  <b>demo-app-quarkus-manifests</b> · Kubernetes deployment manifests for the demo Quarkus application, managed with <a href="https://argo-cd.readthedocs.io/">Argo CD</a> on the <a href="https://kubezt.com">KubeZT</a> platform.
</p>

<p align="center">
  <img alt="Kubernetes badge" src="https://img.shields.io/badge/Kubernetes-1.31+-blue?logo=kubernetes">
  <img alt="Argo CD badge" src="https://img.shields.io/badge/GitOps-Argo%20CD-orange?logo=argo">
  <img alt="Platform badge" src="https://img.shields.io/badge/Platform-KubeZT-4B0082?logo=kubernetes">
  <img alt="License" src="https://img.shields.io/badge/license-Apache--2.0-green">
</p>

---

## 📦 Overview

This repository contains the **Kubernetes manifests** for deploying the [`demo-app-quarkus`](https://github.com/your-org/demo-app-quarkus) application onto a KubeZT cluster.

It includes:

- Namespace definition (`namespace.yaml`)
- Private registry image pull secret (`regcred.yaml`)
- Deployment spec (`deployment.yaml`)
- Internal ClusterIP service (`service.yaml`)

---


## 🛠 Project Structure

```
demo-app-quarkus-manifests/
└── manifests/
    ├── namespace.yaml     # Defines the 'demo' namespace with pod security labels
    ├── regcred.yaml       # Secret for pulling images from a private registry (Harbor)
    ├── deployment.yaml    # Kubernetes Deployment for the Quarkus application
    └── service.yaml       # ClusterIP Service exposing port 8080
```

## 📁 Files

| File             | Description |
|------------------|-------------|
| `namespace.yaml` | Creates the `demo` namespace with privileged pod security |
| `regcred.yaml`   | ImagePullSecret for Harbor registry authentication |
| `deployment.yaml`| Deploys the Quarkus app using a fixed image digest |
| `service.yaml`   | Exposes the app internally on port 8080 |

All manifests are fully self-contained and namespace-scoped.

---

## 🚀 Manual Deployment (Dev/Test)

Apply the manifests in order:

```bash
kubectl apply -f manifests/namespace.yaml
kubectl apply -f manifests/regcred.yaml
kubectl apply -f manifests/deployment.yaml
kubectl apply -f manifests/service.yaml
```

---

## 🤖 Argo CD Integration

These manifests are designed to work seamlessly with Argo CD:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: demo-app-quarkus
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://git.kubezt.svc.dev.kubezt/KubeZT/demo-app-quarkus-manifests.git
    targetRevision: HEAD
    path: manifests
  destination:
    server: https://kubernetes.default.svc.dev.kubezt
    namespace: demo
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
    syncOptions:
      - CreateNamespace=true
```

This enables **GitOps-based deployment** with automatic syncing, healing, and pruning.

---

## 🛡️ Secure by Default

These manifests follow KubeZT platform security practices:

- No public service exposure
- Private registry with signed image digests
- Minimal container with no OS packages or shell
- Namespace isolation and pod security labels

---

## 📚 Related Repositories

- [`demo-app-quarkus`](https://github.com/KubeZT/demo-app-quarkus): Source code and Nix build for the Quarkus app

---

## 🤝 About KubeZT

KubeZT is a hardened, zero trust Kubernetes platform designed for secure software delivery in tactical, resource constrained environments.

Learn more at [https://KubeZT.com](https://kubezt.com)

---

## 📜 License

This project is licensed under the [Apache 2.0 License](./LICENSE).
