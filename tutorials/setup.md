# Introduction

## Let's get started!

This guide will take you through configuring access to a GKE cluster for upcomming labs

**Time to complete**: About 5 minutes

Click the **Start** button to move to the next step.


## Setup Env
start by setting the project id
```bash
export PROJECT_ID=<id>
export NS="${USER}-${RANDOM}"
export CLUSTER_NAME=istio-demo
export CLUSTER_REGION=europe-west1-b
```

now run the script to save the config for later 
```bash
#!/usr/bin/env bash

  cat > ~/demo-configuration <<EOL

export PROJECT_ID=$PROJECT_ID
export CLUSTER_NAME=$CLUSTER_NAME
export CLUSTER_REGION=$CLUSTER_REGION

gcloud config set project $PROJECT_ID

gcloud container clusters get-credentials --zone="${CLUSTER_REGION}" "${CLUSTER_NAME}"
export INGRESS_HOST="$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')"

export NS="${NS}"
kubectl config set-context "$(kubectl config current-context)" --namespace="${NS}"

export NAME=$(echo "${USER//_/-}")

EOL
```


## try the to set the config in the shell

```bash
. ~/demo-configuration
```


## Install istioctl

```bash
curl -sL https://istio.io/downloadIstioctl | sh -
```

```bash
echo "export PATH=$PATH:$HOME/.istioctl/bin" >> ~/.bashrc
```

```bash
export PATH=$PATH:$HOME/.istioctl/bin
```

Continue on to the **next** step



## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

start the **next** tutorial isito basics
```bash
cloudshell launch-tutorial -d tutorials/istio-1-basics.md
```
