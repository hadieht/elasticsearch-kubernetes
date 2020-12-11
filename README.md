# ElasticSearch Deployment on Kubernetes
This project contains the manifests that are used to deploy highly available Elasticsearch cluster.

Kubernetes version is v1.16.1 and Docker version is 18.9.2 for all of the worker nodes.

### Advantages in this project

Highly Available and Scalable Elasticsearch on Kubernetes

### Deploy

This is a step by step guide for the deployment of Elasticsearch on top of Kubernetes using the content provided in this repository. 

###### kubernetes

```SHELL

kubectl create -f elasticdata-pv.yaml
kubectl create -f elasticdata-pvc.yaml

kubectl create -f elasticsearch-client.configmap.yaml
kubectl create -f elasticsearch-client.service.yaml
kubectl create -f elasticsearch-client.deployment.yaml

kubectl create -f elasticsearch-data.configmap.yaml
kubectl create -f elasticsearch-data.service.yaml
kubectl create -f elasticsearch-data.replicaset.yaml

kubectl create -f elasticsearch-master.configmap.yaml
kubectl create -f elasticsearch-master.deployment.yaml
kubectl create -f elasticsearch-master.service.yaml

```

### Verification
1. verify the statefulset has been created and all the pods are alive ```kubectl get statefulset -w```
	- use ```-w``` to watch changes that occur on the statefulset; 
	- keep watching until the current number of pods is ```3```; 
	- once the current number of pods is 3, we have 3 elasticsearch pods that are up and running with the names ```elasticsearch-$i``` where $i={0,1,2}

2. to check the logs for a given pod use ```kubectl logs elasticsearch-$i```
	- use ```-f``` to keep tracking the logs for the given pod
	
3. given the statefulset is created, is absolute verification that the headless-service is created properly and is properfly referenced by the elasticsearch nodes

4. verify the cluster-ip and load-balancer services are created; ```kubectl get svc``` 
	- the cluester-ip service is used for internal access 
    - the load-balancer service is used for external access, through the provided EXTERNAL-IP


```
