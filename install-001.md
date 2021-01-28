# Installing and Testing the Components of a Kubernetes Cluster

## Introduction

We have been given three nodes, in which we must install the components necessary to build a running Kubernetes cluster. Once the cluster has been built and we have verified all nodes are in the ready status, we need to start testing deployments, pods, services, and port forwarding, as well as executing commands from a pod.

## Solution
Log in to all three servers using the credentials on the lab page (either in your local terminal, using the Instant Terminal feature, or using the public IPs), and work through the objectives listed.

### Get the Docker gpg, and add it to your repository.
1. In all three terminals, run the following command to get the Docker gpg key:

        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  
2. Then add it to your repository:

            sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  
### Get the Kubernetes gpg key, and add it to your repository.

1. In all three terminals, run the following command to get the Kubernetes gpg key:

            curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

2. Then add it to your repository:

            cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
            deb https://apt.kubernetes.io/ kubernetes-xenial main
            EOF

3. Update the packages:

            sudo apt update

### Install Docker, kubelet, kubeadm, and kubectl.
1. In all three terminals, run the following command to install Docker, kubelet, kubeadm, and kubectl:

        sudo apt install -y docker-ce=5:19.03.10~3-0~ubuntu-focal kubelet=1.18.5-00 kubeadm=1.18.5-00 kubectl=1.18.5-00

### Initialize the Kubernetes cluster.
1. In the Controller server terminal, run the following command to initialize the cluster using kubeadm:

        sudo kubeadm init --pod-network-cidr=10.244.0.0/16

### Set up local kubeconfig.
1. In the Controller server terminal, run the following commands to set up local kubeconfig:

        sudo mkdir -p $HOME/.kube

        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

        sudo chown $(id -u):$(id -g) $HOME/.kube/config
        
### Apply the flannel CNI plugin as a network overlay.
1. In the Controller server terminal, run the following command to apply flannel:

        kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml

### Join the worker nodes to the cluster, and verify they have joined successfully.
1. When we ran sudo kubeadm init on the Controller node, there was a kubeadmin join command in the output. You'll see it right under this text:

You can now join any number of machines by running the following on each node as root:
To join worker nodes to the cluster, we need to run that command, as root (we'll just preface it with sudo) on each of them. It should look something like this:

        sudo kubeadm join <your unique string from the output of kubeadm init>
        
### Run a deployment that includes at least one pod, and verify it was successful.
1. In the Controller server terminal, run the following command to run a deployment of ngnix:

        kubectl create deployment nginx --image=nginx

2. Verify its success:

        kubectl get deployments
        
### Verify the pod is running and available.
1. In the Controller server terminal, run the following command to verify the pod is up and running:

        kubectl get pods
        
        


