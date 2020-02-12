# Introduction

## Let's get started!

This guide will take you through setting up a GKE cluster and installing istio for the upcomming labs with istio

**Time to complete**: About 10 minutes

Click the **Start** button to move to the next step.


## Setup Env
start by setting some environment variables in the shell

```
#!/usr/bin/env bash

  cat > ~/demo-configuration <<EOL

export PROJECT_ID=$PROJECT_ID
export CLUSTER_NAME=istio-demo
export CLUSTER_REGION=europe-west1-b

gcloud config set project $PROJECT_ID
export INGRESS_HOST="$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')"; echo "$INGRESS_HOST"

EOL
```


## Name of step
Continue on to the **next** step



## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

start the **next** tutorial <set name>
```bash
cloudshell launch-tutorial -d tutorials/istio-<int>-<name>.md
```
