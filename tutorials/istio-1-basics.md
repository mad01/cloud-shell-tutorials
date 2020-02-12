# Introduction


## Let's get started!

This guide will take you through installing a nginx service. configuring it to be 
a member of the istio mesh, and calling it using the default istio edge proxy

**Time to complete**: About 5 minutes

Click the **Start** button to move to the next step.


## Setup Env
start by setting some environment variables in the shell from the 
config we created in the setup step. If you haven't done this yet 
start that tutorial with the following command in cloud shell
`cloudshell launch-tutorial -d tutorials/setup.md`

If all that is set you can go ahead and use that config
```bash
. ./demo-configuration
```

Continue on to the **next** to get started


## Creating the namespace and enable istio
To enable istio automatic sidecar injection we need to set a label
on the namespace `istio-injection=enabled`. By adding this label
istio will inject the `istio-proxy` is that needed to become a
member of the istio mesh

lets first create the namespace for the tutorial
```bash
kubectl create ns $NS
```
now lets add the label on the namespace
```bash
kubectl label namespace $NS istio-injection=enabled
```

lets now set the current k8s context to the namespace basics
```bash
kubectl config set-context "$(kubectl config current-context)" --namespace=$NS
```

Continue on to the **next** step



## Deploying the Nginx service
We will start by deploying a normal k8s `deployment` `service` that is the 
nginx service

lets take a look at the k8s deployment file first 
```bash
cat k8s/basics/deploy.yaml
```

now lets deploy this to the cluster
```bash
kubectl apply -f k8s/basics/deploy.yaml -n $NS
```

Continue on to the **next** step



## Apply the istio Gateway

lets take a look at the istio gateway manifest
```bash
cat k8s/basics/gw.yaml
```
now lets apply it
```bash
kubectl apply -f k8s/basics/gw.yaml -n $NS
```

Continue on to the **next** step



## Try calling the nginx
Lets try to call the istio service before we add it as a member in the mesh
```bash
curl -v --header 'Host: $NAME.example.com' $INGRESS_HOST/
```

spoiler it will fail 

Continue on to the **next** step



## Apply the istio virtual service
first lets prep the virtual service with a unique name. 
we will just your username in the shell to make it easy.

```bash

sed -i -e "s/{{NAME}}/${NAME}/g;" \
    k8s/basics/vs.yaml
```

lets take a look at the istio virtual service manifest
```bash
cat k8s/basics/vs.yaml
```
now lets apply it
```bash
kubectl apply -f k8s/basics/vs.yaml -n $NS
```

Continue on to the **next** step


## Try calling the nginx now again when a member
lets now try to call the service again when we have configured 
it to be a member of the isito mesh

```bash
curl -v --header 'Host: $NAME.example.com' $INGRESS_HOST/
```

Continue on to the **next** step


## Cleanup
We are now done with the current tutorial so lets cleanup

```bash
kubectl delete ns $NS
```

```bash
git checkout .
```

Continue on to the **next** step




## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You’re all set to start the upcomming istio guides!

start the **next** tutorial <set name>
```bash
cloudshell launch-tutorial -d tutorials/istio-<int>-<name>.md
```
