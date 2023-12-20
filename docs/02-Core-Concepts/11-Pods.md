# Pods
  - Take me to [Video Tutorial](https://kodekloud.com/topic/pods-2/)
  
In this section, we will take a look at PODS.
- POD introduction
- How to deploy pod?

#### Kubernetes doesn't deploy containers directly on the worker node.

  ![pod](../../images/pod.PNG)
  
#### Here is a single node kubernetes cluster with single instance of your application running in a single docker container encapsulated in the pod.

![pod1](../../images/pod1.PNG)

#### Pod will have a one-to-one relationship with containers running your application.

  ![pod2](../../images/pod2.PNG)
  
## Multi-Container PODs
- A single pod can have multiple containers except for the fact that they are usually not multiple containers of the **`same kind`**.
  
  ![pod3](../../images/pod3.PNG)
  
## Docker Example (Docker Link)
  
  ![pod4](../../images/pod4.PNG)
  
## How to deploy pods?
Lets now take a look to create a nginx pod using **`kubectl`**.

- To deploy a docker container by creating a POD.
  ```
  $ kubectl run nginx --image nginx
  ```

- To get the list of pods
  ```
  $ kubectl get pods
  ```

 ![kubectl](../../images/kubectl.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/pods/pod/
- https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/


My Notes:

Before we head into understanding pods,

we would like to assume that the following have been set up already.

At this point, we assume that the application is already developed and built into Docker images, and it is available on a Docker repository like Docker Hub so Kubernetes can pull it down.
We also assume that the Kubernetes cluster has already been set up and is working. This could be a single-node setup or a multi-node setup. Doesn't matter. All the services need to be in a running state. As we discussed before, with Kubernetes, our ultimate aim is to deploy our application in the form of containers on a set of machines that are configured as worker nodes in a cluster. However, Kubernetes does not deploy containers directly on the worker nodes. The containers are encapsulated into a Kubernetes object known as pods.
A pod is a single instance of an application. A pod is the smallest object that you can create in Kubernetes.
Here we see the simplest of simplest cases where you have a single-node Kubernetes cluster with a single instance of your application running in a single Docker container encapsulated in a pod.
What if the number of users accessing your application increase and you need to scale your application?
You need to add additional instances of your web application to share the load.
Now, where would you spin up additional instances?
Do we bring up new container instance within the same pod? No, we create new pod altogether with a new instance of the same application.
As you can see, we now have two instances of our web application running on two separate pods on the same Kubernetes system or node. What if the user base further increases and your current node has no sufficient capacity?
Well, then you can always deploy additional pods on a new node in the cluster.
You will have a new node added to the cluster to expand the cluster's physical capacity.
So what I'm trying to illustrate in this slide is that pods usually have a one-to-one relationship with containers running your application. To scale up, you create new pods, and to scale down, you delete existing pod.
You do not add additional containers to an existing pod to scale your application.
Also, if you're wondering how we implement all of this and how we achieve load balancing between the containers et cetera, we will get into all of that in a later lecture.
For now, we are only trying to understand the basic concepts.

We just said that pods usually have a one-to-one relationship with the containers, but are we restricted to having a single container in a single pod?
No, a single pod can have multiple containers except for the fact that they're usually not multiple containers of the same kind. As we discussed in the previous slide, if our intention was to scale our application, then we would need to create additional pods, but sometimes you might have a scenario where you have a helper container that might be doing some kind of supporting task for our web application such as processing a user and their data, processing a file uploaded by the user, et cetera and you want these **helper containers** to live alongside your application container.
In that case, you can have both of these containers part of the same pod so that when a new application container is created, the helper is also created, and when it dies, the helper also dies since they're part of the same pod.

The two containers can also communicatewith each other directly by referring to each other as local host since they share the same network space, plus they can easily share the same storage space as well.
If you still have doubts in this topic, I would understand if you did because I did the first time I learned these concepts.
We could take another shot at understanding pods from a different angle.

Let's, for a moment, keep Kubernetes out of our discussion and talk about simple Docker containers.
Let's assume we were developing a process or a script to deploy our application on a Docker host.
Then we would first simply deploy our application using a simple Docker run Python app command and the application runs fine
and our users are able to access it.
When the load increases, we deploy more instances of our application by running the Docker run commands many more times.
This works fine, and we are all happy. Now, sometime in the future, our application is further developed, undergoes architectural changes and grows and gets complex. We now have a new helper container that helps our web application by processing or fetching data from elsewhere. These helper containers maintain a one-to-one relationship with our application container and thus needs to communicate the application containers directly and access data from those containers.
For this, we need to maintain a map of what app and helper containers are connected to each other.
We would need to establish network connectivity between these containers ourselves using links and custom networks.
We would need to create shareable volumes and share it among the containers. We would need to maintain a map of that as well, and most importantly, we would need to monitor the state of the application container and when it dies, manually kill the helper container as well as it's no longer required.
When a new container is deployed, we would need to deploy the new helper container as well.
With pods, Kubernetes does all of this for us automatically.
We just need to define what containers a pod consists of and the containers in a pod by default will have access to the same storage, the same network namespace and same fit as in they will be created together and destroyed together.
Even if our application didn't happen to be so complex and we could live with a single container,
Kubernetes still requires you to create pods, but this is good in the long run as your application is now equipped for architectural changes and scale in the future.
However, also note that multi containers pod are a rare use case, and we are going to stick to single containers per pod
in this course.
Let's us now look at how to deploy pods.
Earlier we learned about the kubectl run command.
What this command really does is it deploys a Docker container by creating a pod, so it first creates a pod automatically
and deploys an instance of the NGINX Docker image, but where does it get the application image from?
For that, you need to specify the image name using the dash dash image parameter.
The application image, in this case, the NGINX image, is downloaded from the Docker Hub repository.
Docker Hub, as we discussed, is a public repository where latest Docker images of various applications are stored.
You could configure Kubernetes to pull the image from the public Docker Hub or a private repository within the organization.
Now that we have a pod created, how do we see the list of pods available?
The kubectl get pods command helps us see the list of pods in our cluster.
In this case, we see the pod is in a container creating state and soon changes to a running state when it is actually running.
Also, remember that we haven't really talked about the concepts on how a user can access the NGINX web server, and so in the current state, we haven't made the web server accessible to external users.
You can, however, access it internally from the node.
For now, we will just see how to deploy a pod, and in a later lecture, once we learn about networking and services,
we will get to know how to make this service accessible to end users.

That's it for this lecture.


