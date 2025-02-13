# Kubernetes Services - ClusterIP
  - Take me to [Video Tutorial](https://kodekloud.com/topic/services-cluster-ip-2/)
  
In this section we will take a look at **`services - ClusterIP`** in kubernetes
         
## ClusterIP
- In this case the service creates a **`Virtual IP`** inside the cluster to enable communication between different services such as a set of frontend servers to a set of backend servers. All the pods have IP address assigned to them but they are not static as the pods can go down anytime and new pods are created all the time and so we cannot rely on these IP addresses for internal communication between the application. What if fiest front end pod needs to connect with backend service, which of the three it would go to and who makes the decision?
    
    ![srvc1](../../images/srvc1.PNG)
    
#### What is a right way to establish connectivity between these services or tiers  
- A kubernetes service can help us group the pods together and provide a single interface to access the pod in a group. For example a service created for the backend pods will help group all the backend pods together and provide a single interface for other pods to access the service. The requests are forwarded to one of the pods under the service randomly. By this microservices based apps can be deployed easily on k8 cluster. 

  ![srvc2](../../images/srvc2.PNG)
  
#### To create a service of type ClusterIP
```
apiVersion: v1
kind: Service
metadata:
 name: back-end
spec:
 types: ClusterIP
 ports:
 - targetPort: 80
   port: 80
 selector:
   app: myapp
   type: back-end
```
```
$ kubectl create -f service-definition.yaml
```

#### To list the services
```
$ kubectl get services
```
  ![srvc3](../../images/srvc3.PNG)
   
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/
