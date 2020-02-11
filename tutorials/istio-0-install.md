# Introduction


## Let's get started!

This guide will take you through setting up a GKE cluster and installing istio for the upcomming labs with istio

**Time to complete**: About 15 minutes

Click the **Start** button to move to the next step.


## Setup Env
start by setting some environment variables in the shell

```
export PROJECT_ID=<id>
export CLUSTER_NAME=<name>
```

Continue on to the next step


## Install istioctl

```bash
curl -sL https://istio.io/downloadIstioctl | sh -
```

```bash
echo "export PATH=$PATH:$HOME/.istioctl/bin" >> ~/.bashrc
source ~/.bashrc
```

Continue on to the next step


## Create GKE cluster

```bash
gcloud beta container \
    --project "${PROJECT_ID}" clusters create "${CLUSTER_NAME}" \
    --zone "europe-west1-d" \
    --release-channel "regular" \
    --machine-type "n1-standard-2" \
    --preemptible \
    --num-nodes "2" \
    --enable-stackdriver-kubernetes \
    --enable-autoscaling \
    --min-nodes "2" \
    --max-nodes "8" \
    --enable-autoupgrade \
    --enable-autorepair
```

Continue on to the next step


## Deploy Istio

generate manifests
```bash
istioctl manifest generate \
    --set "profile=default" \
    --set "values.kiali.enabled=true" \
    --set "values.kiali.dashboard.grafanaURL=http://grafana:3000"  \
    --set "values.global.disablePolicyChecks=false" \
    --set values.global.proxy.accessLogFile="/dev/stdout" \
    --set "values.grafana.enabled=true" > deploy.yaml
```

deploy the generated manifests (might need to apply more then one time since it contains CRDs. totally safe to do)
just run apply until clean apply
```bash
kubectl apply -f deploy.yaml
```

create secret for the kiali portal
```bash
kubectl create secret generic kiali \
    --namespace istio-system \
    --from-literal "username=admin" \
    --from-literal "passphrase=admin"
```

Continue on to the next step




## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You’re all set to start the upcomming istio guides!
