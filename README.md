# Argo CD APP Config

This repo holds just the deployment configuration for the argo cd. The actual source code for the applications are managed in a different repo.

## Requirements

The minimum requirement is a kubernetes cluster.


Now, in case if you do not have a kubernetes cluster you can test the whole flow using Minikube by using the docker driver. So you many need to install docker desktop and then install minikube using the docker driver.

## Getting Started

Before we get started make sure to start the minikube and check its status with the below commands

```bash
minikube start
minikube status
```

Its a good practice to create a seperate namespace for argo cd and install the tool inside this namespace. read more [here](https://argo-cd.readthedocs.io/en/stable/?_gl=1*12j3r4i*_ga*NTI0ODY3MzgxLjE3MTA5MDc3Nzk.*_ga_5Z1VTPDL73*MTcxMzAwMTM0Ny4yLjAuMTcxMzAwMTM0OS4wLjAuMA..#getting-started)

1. Run the below command to create argocd namespace.

```bash
kubectl create namespace argocd
```

2. Run the below command to install the latest stable argocd tool inside the above namespace.

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Check if all the argocd pods and services were up and running, by running the below commands.

```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
```

4. As Argo CD server runs on ports HTTP & HTTPS. We need to port forward the service to access the web ui on our machine by running the below command and do not kill the shell.

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

5. Now go to the [link](http://localhost:8080/) to access the argo cd webui. The default username is `admin` and the password is auto-generated and stored as a secret with key `argocd-initial-admin-secret` in the namespace, which u can view it by running the below command.

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo
```

## Configuring Argo CD

Write the configuration file for Argo CD to connect to github where our app config files are hosted. This is also checked-in the same repository in a file called `application.yml`.

So just the final time we can do kubectl apply manually for the below command

```bash
kubectl apply -f application.yml
```