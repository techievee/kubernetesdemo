## Kubernetes configurations

The repository contains various strategy for implementing the Kubernetes cluster from Source code.

* Approach 1 (servian-auto.yaml) -  ( Deployment time is ~5 minutes )
    - In this approach, the Docker image is build using the Docker hub automated githooks.  
    - Git hook is configured in the Dockerhub with the Github source repository.
    - As soon as a commit is performed on the repository, the build trigger, triggers the build.
    - The images are build using a "auto" tag, so this image is pulled when using the servian-auto.yaml file.
    - Dockersecrets are not required in the configuration as there is no push of image
 
             git clone https://github.com/techievee/kubernetesdemo.git
             kubectl apply -f servian-auto.yaml
    
* Approach 2 (servian-build.yaml) -   Deployment time is ~30 minute ( more time is taken for angular build)

    - In this approach, the Docker image is build and pushed to the repository during the deployment.
    - This approach is more appropriate in a environment, where we have our own registry for container repo.
 kaniko( docker in docker) image is used for building the container image.
        - In this approach, whenever the container is initialized
        - Kaniko container pulls the source code from the repository
        - Builds the image for the container
        - Pushes the build images to the docker registry using the configured docker credentials
        - All the images build using this method are tagged as "kaniko"
    - The image with the "kaniko" tag is pulled when using the servian-build.yaml file.
    - Dockersecrets are required in this configuration as build image is pushed to registry

This process can also be done using CI/CD Tools automatically

    
                git clone https://github.com/techievee/kubernetesdemo.git
                kubectl apply -f servian-build.yaml
 
## Accessing Web interface of deployed Application

One all the pods are deployed, then type, to see the result

* http://minikube:30111

Where minikube denotes the minikube machine, DNS name

## Test bed configuration

- The kubernetes configuration were tested using the minikube for Windows
- The minikube was configured to use Hyper-V of windows 10, and started using the command
- 4GB memory allocation for the minikube VM.
- minikube should be given a access to Network adapter with external access
- Accessing the minikube using the internal dns name which is autocreated(minikube) from the host.

          minikube -p servian start --vm-driver hyperv --hyperv-virtual-switch "External Switch" --cpus 4 --memory 4096
          

"External switch" is the name of the switch, please refer the name in your Hyper-V network settings.

## Important
* DNS from your host system should be getting resolved using minikube
* Try nslookup minikube

## Ports Exposed as Nodeport
* Golang exposes the API in port : 30112
* Angular exposes the web interface : 30111

## Kubernetes dashboard enabling

The dashboard of the minikube can be enabled by running

     kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
     minikube -p servian dashboard --url

##Kubernetes components and its usages for various purpose:
   * Redis - Deployed using Stateful set
   * Redis Disk - Deployed using the Persistent volume and Persistent Volume Claim
   * Configuration for Golang - Using the ConfigMap
   * Password for Redis, Docker hub - Using Secrets
   * Angular Application - Using the Deployment
   * Golang application - Using the Deployment
   * Access to Golang and Angular - Using the Nodeport service (30111- Angular, 30112- Golang)
   * Access to Redis - No External access, Access via kube-dns( redis name) via clusterip service
   * Building and Pushing container - Using Kaniko image (Running docker in docker) as Onint container or as Jobs

## References
The project uses other 2 project

    https://github.com/techievee/godemo
    https://github.com/techievee/angulardemo
