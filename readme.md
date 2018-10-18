Kubernetes configuration for running the checkers app environment.

# Setup

1. Have access to [`kubectl`](https://kubernetes.io/docs/setup/pick-right-solution/) and [`helm`](https://github.com/helm/helm#install) from your command line
2. If you're running locally (rather than in gcloud, for example), ensure you have docker installed (and possibly minikube depending on your version of docker)
3. Run `helm init` if you haven't installed Tiller on your cluster yet
4. Copy `values.example.yaml` to `values.yaml` and fill in *all* the values for your environment
5. Ensure you have the domain you specified in your system's hosts file pointing at 127.0.0.1
6. Run `helm dependency build` and `helm install .` from the root of this repo
7. After a few moments, the app should be running at your domain

# Running in gcloud

If you already have gcloud on your system and have no problem switching kubeconfigs locally, you should just be able to follow the above instructions and deploy to a GKE cluster.

If you want a slightly more isolated approach, you can head to the `/kubectl` directory where you have access to a container that has kubectl and gcloud already installed.

1. Mount the local path to this repo in a `docker-compose.override.yaml` file.
2. Run `docker-compose up -d` from that directory
3. Run `docker exec -it kubectl bash` to get into the container
4. Head over to `cd /var/kube`
5. Run `gcloud init` and follow instructions
6. Run `gcloud container clusters get-credentials production` (replacing `production` with GKE cluster name)
7. Run any `kubectl` or `helm` command from `/var/kube` (where the repo code is)

## Setting up your cluster with a GPU

When deploying to a cluster that has a GPU node pool, you'll need to follow the [Kubernetes instructions for scheduling GPUs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus). There are a few drivers you'll need to install:

```
# Install NVIDIA drivers on Container-Optimized OS:
kubectl create -f https://raw.githubusercontent.com/GoogleCloudPlatform/container-engine-accelerators/k8s-1.10/daemonset.yaml

# Install NVIDIA drivers on Ubuntu (experimental):
kubectl create -f https://raw.githubusercontent.com/GoogleCloudPlatform/container-engine-accelerators/k8s-1.10/nvidia-driver-installer/ubuntu/daemonset.yaml

# Install the device plugin:
kubectl create -f https://raw.githubusercontent.com/kubernetes/kubernetes/release-1.10/cluster/addons/device-plugins/nvidia-gpu/daemonset.yaml
```

And then you can modify the `values.yaml` file to set `prediction.gpu` to `true`.