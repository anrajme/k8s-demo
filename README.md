# Python flask hello-world application deployment on Kubernetes!
This is a sample Python flask hello world application to demonstrate the application containerisation and orchestration workflow.

![image](https://user-images.githubusercontent.com/1909476/128383766-f788a85e-8fac-4d65-b27a-b87016ab4ad9.png)




### Setup the workspace
1. Clone the repostiory locally

```bash
$ git clone --recursive git@github.com:anrajme/k8s-demo.git
```

### Build the container and upload to the registry
1. Building docker image

```bash
$ docker build -f Dockerfile -t hello-world:v1 .
```

2. Tag the new image

```bash
$ docker image tag hello-world:v1 anraj/hello-world:v1
```

3. Push the image to Docker hub

```bash
$ docker push anraj/hello-world:v1
```

### Create the Kubernetes objects
1. Create kubernetes Deployment

```bash
$ kubernetes create -f deployment.yaml
```

2. Create the Kubernetes service

```bash
$ kubernetes create -f service.yaml
```

### Verify the application

1. Verify the hello-world application by accessing it via the Node's public IP address. Its "localhost" in case if you are using docker-desktop

```bash
$ curl http://locahost:30000
```

### Upgrade the application and test the deployment rollout

1. Edit main.py to print "Hello world V2!"

2. Follow the above steps to build a new image and tag it as hello-world:v2

```bash
$ docker build -f Dockerfile -t hello-world:v2 .
```

```bash
$ docker image tag hello-world:v2 anraj/hello-world:v2
```

```bash
$ docker push anraj/hello-world:v2
```

3. Update the Docker image to rollout the new version

```bash
$ kubectl set image deployment/hello-world hello-world=anraj/hello-world:v2
```

4. Verify the rollout status

```bash
$ kubectl rollout history deployment/hello-world
```

5. Verify the new application Version

```bash
$ curl http://locahost:30000
```

6. Rollback the application to the previous version and verify

```bash
$ kubectl rollout undo deployment/hello-world
```

```bash
$ kubectl rollout status deployment/hello-world
```

```bash
$ curl http://locahost:30000
```

