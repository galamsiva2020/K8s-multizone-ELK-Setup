
# Three Zone Highly Available ElasticSearch Deployment on Kubernetes Cluster

## Steps

1. Install and optionally customize the local-path storage class as we are using local SSDs for storage and we don't want to manually create PVs.

2. Label nodes to indicate which zone they are present in. We have three zones {a, b, c} and three nodes with 3 in each zone.

``` 
kubectl label node node1 zone=a
kubectl label node node2 zone=b
kubectl label node node3 zone=c
```

3. Label nodes to indicate which role they would support, i.e. Elastic data, master etc.

```
kubectl label node node1 node2 node3 es-data=true
kubectl label node node1 node2 node3  es-master=true
```
prerequisite:
a) Create a StorageClass "local-path" by following below cmd:
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml

4. From within the multizone directory, just execute:

``` 
kubectl create ns elk
#please run the below yaml as per the sequence

kubectl -n elk create -f es-config.yaml
kubectl -n elk create -f es-data-stateful-a.yaml
kubectl -n elk create -f es-data-stateful-b.yaml
kubectl -n elk create -f es-data-stateful-c.yaml

kubectl -n elk create -f es-ingest.yaml

kubectl -n elk create -f es-kibana-config.yaml
kubectl -n elk create -f es-kibana.yaml
kubectl -n elk create -f es-svc.yaml

kubectl -n elk create -f es-master-config.yaml
kubectl -n elk create -f es-master-stateful.yaml

```
8. Watch for pods as they come along
 
```
kubectl -n elk  get pods -o wide
```
