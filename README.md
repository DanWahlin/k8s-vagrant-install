# Setting up a Kubernetes Cluster using Vagrant

This will create a development/test 3 node Kubernetes 1.26.* cluster that runs in VirtualBox.

## Requirements

- [Vagrant](https://www.vagrantup.com/)
- [VirtualBox](https://www.virtualbox.org/)
- [Ubuntu](https://ubuntu.com/download)

## Install Steps

Please note that this has only been tested when run from Ubuntu. Your mileage may vary on other distributions.

1. Start up your Ubuntu instance and login.

1. Clone this repo into your Ubuntu instance.

1. Go into cloned project's folder and run:

    ``` 
    vagrant up
    ```

1. After the master and 2 worker nodes are created, run the following to ssh into the master:

    ```
    vagrant ssh master
    ```

1. Run the following command to initialize Kubernetes and support joining worker nodes:

    ```
    sudo kubeadm init --apiserver-advertise-address 192.168.33.13 --pod-network-cidr=192.168.0.0/16

    # Run the commands shown in the console output. Example:

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

    # Copy the join command and save it (you'll need it later). 
    # DO NOT USE THIS EXACT COMMAND - it's just an example :-)

    kubeadm join 192.168.33.13:6443 --token xi0awh.adpuyyyafftlzg50 \
	--discovery-token-ca-cert-hash sha256:692b16520d2cb2310aef597ea2b68509aca9f78086b28dc16363ef54bb932f55 

    ```

1. Install a Calico network (steps in the link below - select the `Manifest` tab):

    https://projectcalico.docs.tigera.io/getting-started/kubernetes/self-managed-onprem/onpremises


    ```
    # Example (use the calico version from the above link):

    curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml -O

    kubectl apply -f calico.yaml

    ```

1. Join the worker nodes to the master node:

    ```
    vagrant ssh [worker-1 | worker-2]

    # Run the join command you saved from the master
    ```

1. You can update the machine packages by running the following:

    ```
    sudo apt update
    sudo apt upgrade

1. You should now have a 3 node cluster (1 master and 2 workers):

    * 192.168.33.13 master
    * 192.168.33.14 worker-1
    * 192.168.33.15 worker-2

## Credits 

The Vagrant file is from a project originally created by Mehmet Ali Baykara:

https://github.com/mbaykara/k8s-cluster
