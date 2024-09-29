# Kubernetes Technical Challenge
Deploy Microservices system on Kubernetes environment.
Regarding the system, use Nodejs Molecular as an example, this is a Framework used to build applications with Microservices architecture.
## Requirements
   * A [GitHub repositories](https://github.com/nholuongut/microservices) 
   * A [Docker Hub Images](https://hub.docker.com/) 
   * A [Nodejs Molecular](https://moleculer.services/docs/0.14/)
     
   * ### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
  
 ***Note***. Here I am defaulting you have installed docker, kubernetes, kubectl, NGINX Ingress on your system server and have gone internet.
   
## Solution Overview

<img width="468" alt="image" src="https://user-images.githubusercontent.com/58627821/236199958-690f4e7c-ffc5-48d3-b90a-7a9303370fc8.png">

## Brief summary of the architecture of the Microservices system:

-	API Gateway: acts as the application's input, it will create an HTTP Endpoint and receive requests from users.
-	NATS: acts as a relay station so that services can communicate with each other.
-	Categories Service and News Service: this is the Service that performs CRUD work for related resources.
-	Redis: used to cache the results retrieved from the Database, helping to reduce the number of times to execute queries to the DB and increase the speed of the application.
-	Database: PostgreSQL is the place to store data

<img width="468" alt="image" src="https://user-images.githubusercontent.com/58627821/236200547-0680b6b9-ecad-49e9-9d77-1d1026f98e16.png">

## The main components that will be written for each of the above components and deployed to Kubernetes include the followin

# 1.	Building Image.
-	Building Container Image
# 2.	Deployment.
-	Deploy API Gateway
-	Deployment Redis
-	Deployment Database
-	Deployment Categories Service and News Service
-	Deployment NATS

***Note***. General configuration declaration, Use ConfigMap to make the configuration file easy to update. File named microservice-cm.yaml

## The steps are as follows:
# 1.	Building Container Image
- I build Image from NodeJS Microservices Source then PUSH to Docker Hub
# 2.	Deployment
### Deploy API Gateway
-	Create a file called api-gateway-deployment.yaml
-	In Image nluong19/microservices, which I have built above, there are 3 Services in Image which are api, categories, news. Select the Service to run by passing the name of the Service through the SERVICES environment variable, in the configuration file above running the API Gateway should pass the value api.
-	Next step, create Deployment
```
$ kubectl apply -f api-gateway-deployment.yaml
deployment.apps/api-gateway created
$ kubectl get deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE 
api-gateway   0/1     1            0           100s
```
### Deployment Redis 
-	Create a file called redis-deployment.yaml
```
$ kubectl apply -f redis-deployment.yaml
deployment.apps/redis created
$ kubectl get deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
api-gateway   0/1     1            0           16m
redis         1/1     1            1           14s
```

-	Next create a file called redis-service.yaml to connect to this Redis Pod
```
$ kubectl apply -f redis-service.yaml
service/redis created
```

### Deployment Database
-	Create a file called postgres-statefulset.yaml
```
$ kubectl apply -f postgres-service.yaml
service/postgres created
```
 ***Note***. Now when I list the Pod, I see that API Gateway has run successfully.

### Deployment Categories Service and News Service
-	Create a file called categories-news-deployment.yaml
```
$ kubectl apply -f categories-news-deployment.yaml
deployment.apps/categories-service created
deployment.apps/news-service created
```

-	After creating these two Services, in order for them and API Gateway to communicate with each other, it is necessary to create NATS.

### Deployment NATS
-	Create a file called nats-deployment.yaml 
```
$ kubectl apply -f nats-deployment.yaml
deployment/nats created
```

-	Next, create a Kubernetes Service for NATS, create a nats-service.yaml file
```
$ kubectl apply -f nats-service.yaml
service/nats created
```

-	Update all Deployment.
```
$ kubectl apply -f kubectl apply -f api-gateway-deployment.yaml
deployment.apps/api-gateway configured

$ kubectl apply -f categories-news-deployment.yaml
deployment.apps/categories-service configured
deployment.apps/news-service configured
```

-	List the Pods and check if the Services can communicate with each other
```
$ kubectl get pod
NAME                                  READY   STATUS    RESTARTS   AGE
api-gateway-6cb4c6b657-tlkzq          1/1     Running   0          36s
categories-service-689cdb6c6d-gqtlb   1/1     Running   0          20s
nats-65687968fc-2drwp                 1/1     Running   0          20m
news-service-6b85f99987-dcplv         1/1     Running   0          20s
postgres-0                            1/1     Running   0          48m
redis-58c4799ccc-qhv2z                1/1     Running   0          72m
```
### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com) 

![](https://i.imgur.com/waxVImv.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c) 2024
