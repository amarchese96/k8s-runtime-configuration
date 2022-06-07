### Build and deploy the communication aware scheduler

After setting up a Kubernetes cluster, execute the following steps inside the *angelo1996/scheduler-plugins* project to build the communication-aware scheduler:

- Build scheduler docker image (inside scheduler-plugins folder)
```
make local-image LOCAL_REGISTRY=docker.io/angelo1996 LOCAL_IMAGE=kube-scheduler:latest
```

- Push to remote registry
```
docker push docker.io/angelo1996/kube-scheduler:latest
```

Execute the following steps inside this project to deploy the communication-aware scheduler:

- Deploy scheduler with Helm
```
helm install nascheduler scheduler/
```

- Clean up
```
helm uninstall nascheduler
```