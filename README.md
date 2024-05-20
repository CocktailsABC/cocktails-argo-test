# ArgoCD Test Application

This repository contains the Kubernetes manifests and ArgoCD configuration needed to deploy a simple Nginx application. 

## Table of Contents

- [Introduction](#introduction)
- [Application Manifests](#application-manifests)
- [ArgoCD Application Manifest](#argocd-application-manifest)
- [Deployment Steps](#deployment-steps)

## Introduction

This repository demonstrates the use of ArgoCD to deploy and manage a simple Nginx application on a Kubernetes cluster. ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.

## Application Manifests

The following Kubernetes manifests are included in this repository:

### Deployment

The `deployment.yaml` file defines a simple Nginx deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25.5
        ports:
        - containerPort: 80
```

### Service

The `service.yaml` file defines a service to expose the Nginx deployment:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

## ArgoCD Application Manifest

The `argocd-application.yaml` file defines an ArgoCD application to manage the deployment of the Nginx application:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx-app
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: .
    repoURL: 'https://github.com/CocktailsABC/cocktails-argo-test/'
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## Deployment Steps

Follow these steps to deploy the Nginx application using ArgoCD:

1. **Clone the Repository**

    ```bash
    git clone https://github.com/CocktailsABC/cocktails-argo-test.git
    cd cocktails-argo-test
    ```

2. **Push the Manifests to the Git Repository**

    ```bash
    git init
    git add deployment.yaml service.yaml argocd-application.yaml
    git commit -m "Initial commit for ArgoCD test application"
    git remote add origin https://github.com/CocktailsABC/cocktails-argo-test.git
    git push -u origin master
    ```

3. **Apply the ArgoCD Application Manifest**

    ```bash
    kubectl apply -f argocd-application.yaml
    ```

4. **Access the ArgoCD UI**

    Open the ArgoCD UI in your browser and log in. You can then sync the application to deploy it to your Kubernetes cluster.

## Conclusion

With these steps, you have set up a simple deployment pipeline using ArgoCD. This repository contains all the necessary manifests to deploy a test Nginx application and manage it through ArgoCD. Feel free to customize and expand upon this example to suit your needs.
