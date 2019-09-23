## Kubernetes configurations

The repository contains various strategy for implementing the Infrastructure from Source code.

* Approach 1
    - The docker image are build using the GITHOOK, as the source code is pushed, the docker image is build using the tag auto
    - The kubnernetes used the auto tag image to set up the kube cluster
   
   kubectl apply -f https://github.com/techievee/kubernetesdemo/servian-auto.yaml
   
* Approach 2
    - The docker image are build using the Kaniko docker container during the init of container, the docker image is build and pushed using the tag kaniko
    - The kubnernetes used the kaniko tag image to set up the kube cluster
   
   kubectl apply -f https://github.com/techievee/kubernetesdemo/servian-build.yaml

## Test bed configuration

One all the pods are deployed, then type, to see the result

* http://minikube:30111

## Test bed configuration

The kubernetes configuration were tested using the minikube for Windows
The minikube was configured to use Hyper-V of windows 10, and started using the command

minikube -p servian start --vm-driver hyperv --hyperv-virtual-switch "External Switch" --cpus 4 --memory 4096

"External switch" is the name of the switch, please refer the name in your Hyper-V network settings.

## Important
* DNS from your host system should be getting resolved using minikube
* Try nslookup minikube

## Important
* Golang exposes the API in port : 30112
* Angular exposes the web interface : 30112

## Kubernetes dashboard enabling

The dashboard of the kubernetes can be enabled by running

* kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
* minikube -p servian dashboard --url

## References
The project uses other 2 project

    https://github.com/techievee/godemo
    https://github.com/techievee/angulardemo
