This project shows how to run a local Kubernetes cluster with Kind on Docker inside WSL2. Docker Desktop is not used!

See documentation
* https://kind.sigs.k8s.io/
* https://kind.sigs.k8s.io/docs/user/configuration/#extra-port-mappings

### 1. Build the Docker image
See the [spring-boot-docker](https://github.com/sgenette/spring-boot-docker) project. It provides a sample Spring Boot project on Docker exposing a REST end point.

### 2. Create Kind cluster
Create a cluster with exta port configuration. This is required to expose the port for the REST end point to an application, such as a web browser, running on __Windows__.

From the conf directory:

```console
$ kind create cluster --config=cluster.yml
```

The extra port is visible by running the command to list the Docker containers:

```console
$ docker ps
```

Without the extra port configuration, it is still possible to access the REST end point through the _NodePort_ service which is accessible only from __WSL2__ on the internal IP address of the node.

```console
$ kubectl get node kind-control-plane -o wide
$ curl 172.18.0.2:5000/greeting
```

### 3. Load the Docker image onto the cluster

```console
$ kind load docker-image sgenette/spring-boot-docker:latest
```

### 4. Deploy pod and service
From the conf directory:

```console
$ kubectl apply -f pod.yml
$ kubectl apply -f service.yml
```

### 5. Test
Open a web browser in Windows and navigate to http://localhost:5000/greeting?name=Simon