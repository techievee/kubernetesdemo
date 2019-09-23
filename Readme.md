## Kubernetes configurations

The repository contains various strategy for implementing the Infrastructed from Source code.

## Test bed configuration

The kubernetes configuration were tested using the minikube for Windows
The minikube was configured to use Hyper-V of windows 10, and started using the command

minikube -p servian start --vm-driver hyperv --hyperv-virtual-switch "External Switch" --cpus 4 --memory 4096

"External switch" is the name of the switch, please refer the name in your Hyper-V network settings.

## Important
DNS from your host system should be getting resolved using minikube
Try nslookup minikube

## Important
Golang exposes the API in port : 30112
Angular exposes the web interface : 30112

## Kubernetes dashboard enabling

The dashboard of the kubernetes can be enabled by running

* kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
* minikube -p servian dashboard --url

## References
The project uses other 2 project

    https://github.com/techievee/godemo
    https://github.com/techievee/angulardemo
