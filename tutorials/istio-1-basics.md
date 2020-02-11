# Introduction


## Let's get started!

This guide will take you through installing a nginx service. configuring it to be 
a member of the istio mesh, and calling it using the default istio edge proxy

**Time to complete**: About 5 minutes

Click the **Start** button to move to the next step.


## Get GKE cluster credential and ingress IP
The assumption is that you still have `PROJECT_ID` and `CLUSTER_NAME` env from the
itio install step, and that you have a gke cluster with istio installed like in the 
setup. If not go back and to that first

**get credentials**
```bash
gcloud container clusters get-credentials --zone="europe-west1-d" "${CLUSTER_NAME}"
```

**ingress address for istio edge proxy**
```bash
export INGRESS_HOST="$(kubectl -n istio-system get service istio-ingressgateway \
   -o jsonpath='{.status.loadBalancer.ingress[0].ip}')"
echo "$INGRESS_HOST"
```

Continue on to the **next** step



## Creating the namespace and enable istio
To enable istio automatic sidecar injection we need to set a label
on the namespace `istio-injection=enabled`. By adding this label
istio will inject the `istio-proxy` is that needed to become a
member of the istio mesh

lets first create the namespace for the tutorial
```bash
kubectl create ns basics
```
now lets add the label on the namespace
```bash
kubectl label namespace basics istio-injection=enabled
```

lets now set the current k8s context to the namespace basics
```bash
kubectl config set-context "$(kubectl config current-context)" --namespace=basics
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
kubectl apply -f k8s/basics/deploy.yaml
```

Continue on to the **next** step



## Apply the istio Gateway

lets take a look at the istio gateway manifest
```bash
cat k8s/basics/gw.yaml
```
now lets apply it
```bash
kubectl apply -f k8s/basics/gw.yaml
```

Continue on to the **next** step



## Try calling the nginx
Lets try to call the istio service before we add it as a member in the mesh
```bash
curl -v --header 'Host: basics.example.com' $INGRESS_HOST/
```

spoiler it will fail 

Continue on to the **next** step



## Apply the istio virtual service

lets take a look at the istio virtual service manifest
```bash
cat k8s/basics/vs.yaml
```
now lets apply it
```bash
kubectl apply -f k8s/basics/vs.yaml
```

Continue on to the **next** step


## Try calling the nginx now again when a member
lets now try to call the service again when we have configured 
it to be a member of the isito mesh

```bash
curl -v --header 'Host: basics.example.com' $INGRESS_HOST/
```

Continue on to the **next** step


## Cleanup
We are now done with the current tutorial so lets cleanup

```bash
kubectl delete ns basics
```

Continue on to the **next** step




## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You’re all set to start the upcomming istio guides!

start the **next** tutorial <set name>
```bash
cloudshell launch-tutorial -d tutorials/istio-<int>-<name>.md
```
