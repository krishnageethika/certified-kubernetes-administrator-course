# Deployments
  - Take me to [Video Tutorial](https://kodekloud.com/topic/deployments-3/)

In this section, we will take a look at kubernetes deployments
 - Example we have web server that needs to be deployed in a production environment you need not one but many such instances of the web server running for obvious reasons. Secondly whenever newer versions of application builds become available on the docker registry you would like to upgrade your docker instances seamlessly. However you do not want to upgrade all of them at once as we just did which impacts users accessing apps so you many want to upgrade them one after the other which is known as rolling updates. Suppose one of the update resulted in an unexpected error and you are asked to undo the recent change you would like to be able to roll back the changes that were recently carried out .Finally safe for example you would like to make multiple changes during environment such as upgrading the underlying web server versions, scaling  environment and also modifying the resource allocations etc, You donot want to apply each change immediately after the command is running, Instead you would like to apply a pause to your environment, make the changes and then resume so that all the changes are rolled out together. All of these capabilities are available with the k8 deployments. so far in this course we discussed about thoughts which deploy single instances of our application such as the web application in this case. Each container is encapsulated in parts deployed using replication controllers or replica sets and then comes deployment which is k8 object that comes higher in the hierarchy. The deployment provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, pause and resume changes as required. So how do we create a deployment components we first create a deployment the contents of the deployment definition file are exactly similar to the replica set definition file except for the kind which is now going deployment if we walk through the contents of the file it has an API version which is apps/v1.




#### Deployment is a kubernetes object. 
  
 ![deployment](../../images/deployment.PNG)
  
#### How do we create deployment?

```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
- Once the file is ready, create the deployment using deployment definition file
  ```
  $ kubectl create -f deployment-definition.yaml
  ```
- To see the created deployment
  ```
  $ kubectl get deployment
  ```
- The deployment automatically creates a **`ReplicaSet`**. To see the replicasets
  ```
  $ kubectl get replicaset
  ```
- The replicasets ultimately creates **`PODs`**. To see the PODs
  ```
  $ kubectl get pods
  ```
    
  ![deployment1](../../images/deployment1.PNG)
  
- To see the all objects at once
  ```
  $ kubectl get all
  ```
  ![deployment2](../../images/deployment2.PNG)
  
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
