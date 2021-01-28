#Installing and Testing the Components of a Kubernetes Cluster

##Introduction

We have been given three nodes, in which we must install the components necessary to build a running Kubernetes cluster. Once the cluster has been built and we have verified all nodes are in the ready status, we need to start testing deployments, pods, services, and port forwarding, as well as executing commands from a pod.

Solution
Log in to all three servers using the credentials on the lab page (either in your local terminal, using the Instant Terminal feature, or using the public IPs), and work through the objectives listed.

Get the Docker gpg, and add it to your repository.
In all three terminals, run the following command to get the Docker gpg key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
Then add it to your repository:

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
Get the Kubernetes gpg key, and add it to your repository.
In all three terminals, run the following command to get the Kubernetes gpg key:

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
Then add it to your repository:

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
Update the packages:

sudo apt update
