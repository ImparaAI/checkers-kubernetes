Kubernetes configuration for running the checkers app environment.

# Setup

1. Have access to [`kubectl`](https://kubernetes.io/docs/setup/pick-right-solution/) and [`helm`](https://github.com/helm/helm#install) from your command line
2. If you're running locally (rather than in gcloud, for example), ensure you have docker installed (and possibly minikube depending on your version of docker)
3. Run `helm init` if you haven't installed Tiller on your cluster yet
4. Copy `values.example.yaml` to `values.yaml` and fill in *all* the values for your environment
5. Ensure you have the domain you specified in your system's hosts file pointing at 127.0.0.1
6. Run `helm dependency build` and `helm install .` from the root of this repo
7. After a few moments, the app should be running at your domain