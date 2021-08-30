# 6 Tools to Run Kubernetes Locally

Kubernetes is a big and complicated technology and it clearly requires some time and dedication to wrap your head around. There is no vendor lock-in meaning it runs the same no matter which managed cloud platform you use it on. This means using it locally will be no different from using it on the cloud.

There are multiple tools for running Kubernetes on your local machine, but it basically boils down to two approaches on how it is done:

1. running it from a single binary package
2. running it as a container using Docker in Docker (DinD)

### Kubernetes Marketplace
%[https://github.com/alexellis/arkade]

Before we move on to talk about all the tools, it will be beneficial if you installed `arkade` on your machine. It will help you get these tools with a single command.

```bash
curl -sLS https://get.arkade.dev | sudo sh
```

## Features?

All of the tools listed here more or less offer the same feature, including but not limited to:

1. Multi-Node cluster
2. Persistent volumes
3. Networking
4. Certificates
5. Bare-metal support
6. Dashboard
7. Kubernetes Versions
8. Add-ons
9. Cross-platform
10. Tracks upstream Kubernetes

## Single package binary

* ** [k3s](https://github.com/k3s-io/k3s) **
![K3s logo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146388504/EuqE3teij.png)

 k3s is a lightweight Kubernetes distribution from Rancher Labs. It is specifically targeted for running on IoT and Edge devices, meaning it is a perfect candidate for your Raspberry Pi or a virtual machine.

  It comes with a single binary of mere <40 MB and takes as low as 500 MB of RAM.

  You can bootstrap k3s quickly using  [k3sup](https://github.com/alexellis/k3sup) !

  %[https://yankee.dev/multi-node-kubernetes-k3s-locally]

  ```bash
  arkade get k3sup
  ```

* ** [k0s](https://github.com/k0sproject/k0s) **
![k0s.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146395939/RQI9JmCfU.png)
 k0s is the latest entry on the block. By name, you might think it's a more stripped version of k3s, but it's an entirely different distribution from an entirely different company, called Mirantis. Contrary to the name, it comes in a larger binary of 150 MB+.

  It can be run as a binary or in DinD mode. k0s takes security seriously and out of the box, it meets the [FIPS compliance](https://www.sdxcentral.com/security/definitions/what-does-mean-fips-compliant/).  Although, a new distribution, k0s has reached the production-ready status, so there wouldn't be an issue for development usage.

  ```bash
  arkade get k0s
  ```

* ** [Microk8s](https://github.com/ubuntu/microk8s) **
![Micro k8s.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146540551/J5ApDUbjf.jpeg)
 MicroK8s is a Kubernetes distribution by Canonical, the company behind Ubuntu. You already saw this coming; it can only be installed using `snap`.  It comes with loads of add-ons baked in like Fluentd, Grafana and Prometheus.

  If you are on Ubuntu or its derivatives that uses `snap` you'll feel right at home using MicroK8s.

  ```bash
  sudo snap install microk8s --classic
  ```

## Docker in Docker (DinD)

Running Docker inside of Docker (Inception anyone?) is a popular way of bootstrapping Kubernetes. The isolating nature of Docker makes running a multi-node cluster a breeze on a single machine, and also ensures the running instance does not affect the machine itself.

> As the title suggests, you need to have Docker installed on your machine to go this route.

* ** [minikube](https://github.com/kubernetes/minikube) **
![Minikube.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146684436/clc9TZbZ4.png)
  Despite running on top of Docker and similar container technologies, minikube is really flexible in how it operates and supports multiple virtualization drivers making it adaptable to different computing environments.  These include KVM2, Virtualbox, Podman, Hyperkit, Hyper-V and many more.

  ```bash
  arkade get minikube
  ```


* ** [KinD](https://github.com/kubernetes-sigs/kind)**
![Kind.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146781587/SgSgJa0dZ.png)
  Kubernetes in Docker (KinD) is similar to minikube but it does not spawn VM's to run clusters and works only with Docker.  KinD for the most part has the least bells and whistles and offers an intuitive developer experience in getting started with Kubernetes in no time.

  ```bash
  arkade get kind
  ```


* ** [k3d](https://github.com/rancher/k3d/) **
![k3d.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628146849609/XZXhr_4Sk.png)
 k3d is basically running k3s inside of Docker. It provides an instant benefit over using k3s on a local machine, that is, multi-node clusters. Running inside Docker, we can easily spawn multiple instances of our k3s Nodes.

  ```bash
  arkade get k3d
  ```

## Conclusion

Either you choose a single binary package or DinD approach, Kubernetes has made itself pretty accessible. For new learners, the barrier to entry is low, and the feedback loop is instantaneous.

I hope this article has been helpful in deciding which tool to use for running your local Kubernetes instance.

