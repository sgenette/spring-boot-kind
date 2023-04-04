This project shows how to run a local Kubernetes cluster with Kind on Docker inside WSL2. Docker Desktop is not used.

See documentation
* https://kind.sigs.k8s.io/
* https://kind.sigs.k8s.io/docs/user/configuration/#extra-port-mappings

### 1. Build the Docker image
See the [spring-boot-docker](https://github.com/sgenette/spring-boot-docker) project.

### 2. Create Kind cluster
Create a cluster with exta port configuration. This is required to expose the port for the REST end point to an application, such as a web browser, running on Windows.

From the conf directory:

`$ kind create cluster --config=cluster.yml`

### 3. Load the Docker image onto the cluster

`$ kind load docker-image sgenette/spring-boot-docker:latest`

### 4. Deploy pod and service
From the conf directory:

- `$ kubectl apply -f pod.yml`
- `$ kubectl apply -f service.yml`

### 5. Test
Open a web browser in Windows and navigate to http://localhost:5000/greeting?name=Simon