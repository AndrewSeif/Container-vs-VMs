# Container-vs-VMs
Why use Docker?

There are a multitude of reasons that use containers.

Docker is not a container but a runtime, and there are other runtimes like cri-o and containerd, but docker is the most well known.

# Resource Usage:

Since Docker doesnt run its own OS, and instead uses the OS of the host then the footprint for each container is largely compact compared to any VM. (except when you need to run a windows container, which will result in containers of size 5GB+ , thus defeating the purpose of containers)

## Edge Computing
The fact that containers are small in their footprint opens up a whole world of edge computing and processing which delivery better latency to the users. 

## Resource Optimization
Containers by nature are versatile and because they "act" as VMs, means you can run multiple applications that "could use" the same port, but bind the port (lets say 80) to any other port (lets say 33901), which means you can run multiple workloads that use port 80 on the same machine.

docker run -d -p 8080:80 my-web-image-1--> http://your-vm-ip:8080

docker run -d -p 8081:80 my-web-image-2--> http://your-vm-ip:8081

Now combine the 2 above topics and you can run optimized workloads on the edge on a single VM/machine where as with a VM that would have been a nightmare (doable but a nightmare) 

How you might ask? will it will involve alot of iptables and/or port forwarding and a ton load of firewalld. 
# Start Up Time
The startup time difference between a Docker container and a VM is primarily due to the fundamental architectural difference between both of them.

Once requires an OS to start and the other is starts on a running OS, but in case that didnt sell you on it, then lets a step by step of what happens when you start a VM and a Container to run a web server.

## In a VM
- Turn on the VM.
- The VM's BIOS initializes.
- The bootloader loads the VM's OS.
- The OS starts up system services, drivers, etc.
- Finally, the OS starts the web server application.
## In a Container
- Execute `docker run`.
- Docker loads the container's image layers.
- Docker starts the web server process inside the container's isolated environment.

if you have a spare PC you can try starting it, and on your current PC run `docker run hello-world` inside a terminal.
# Dependency Hell
Many times (Specially when you are developing) you would give a working application to an ops engineer or a colleague and they come back and tell you "its not working", your usually response is "but it is working for me, here look!", and that is what we call Dependency Hell.

Chances are the packages you have on your PC right now, is not similar to the person next to you, and even if the packages are the same (JDK for example), it might not be the same version.

To avoid all that hassle, it is easily to build the application, along with its packages in a "container" to make sure that it will work anywhere, on any device, without worrying about "configuring" the VM or download the relevant packages to start the application.

This gives us  "Container Isolation" that is known for containers to have.

Which we will come back to later on.
# Portability
Containers are build by layers, thus making them more portable and easy to move around (aka upload them to Container Registry) or to download and run.

Each command inside a Dockerfile builds a layer which means if for example you are using an [alpine](https://hub.docker.com/_/alpine) to build an image of your application, and you already have alpine on your machine, that means you wont need to download it but rather download the extra needed layers only.

This portability makes it easy to build, run, and move containers across machines. 

# Security (Attack Surface)
Docker Attack Surfaces although known due to many images being opensource, but its very easy to mitigate by making sure to scan your container for vulnerabilities before deploying.

Moreover, you can make sure to run the container in "rootless mode" aka rootless container, to make sure, incase the container was compromised, the attacker will not be able to escalate their privileges and start attacking your infrastructure. 
# Kubernetes
Kubernetes is the defacto container orchestration solution allowing you to deploy thousands of microservices with ease, why is it important? 

Simply because it is faster to deploy containers on kubernetes than the traditional process of deploying apps to web servers, specially in a feature rich industry or a heavily regulated industry where the rate of change is faster than your average deployment.

A couple of industries come to mind.

## Finance
Where you need to be ahead of the competition to providing features at a faster rate, and integrating the clients feedback in your product design process.

## Healthcare
Where you need to protect your customers data (PII) and make sure that whatever new vulnerabilities, bugs and threats are found, are instantly patched. 

# No need for Terraform / Ansible
Containers and Kubernetes by design, remove the purpose and the need to use OpenTofu (Terraform)/Ansible.

You dont need to provision anything, and you dont need to configure anything, simple. 

You can still use these tools to provision some Infra like Load Balancers or Virtual Networks, even configure VMs to run Databases if you opt to deploy them outside of Kubernetes.

But when it comes to containerized workloads, these tools serve no purpose. 
# Gitops
![argocd12](https://github.com/AndrewSeif/Container-vs-VMs/assets/75944452/da00c738-0862-492d-b6e5-fc1ef53d467e)

Basically Gitops is the process of using Git to deploy your infra, where when you commit changes, the process starts to deploy the pushed update.

Gitops is what enables faster deployments and ensure that no config drift will be found between your applications and your infra. 
