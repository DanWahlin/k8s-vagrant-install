# Setting up a Kubernetes Cluster using Vagrant

This will create a development/test 3-node Kubernetes 1.26.* cluster that uses containerd:

* 192.168.33.13 master
* 192.168.33.14 worker-1
* 192.168.33.15 worker-2

## Requirements

- Vagrant
- VirtualBox
- Ubuntu

## Install Steps

Please note that this has only been tested when run from Ubuntu. Your mileage may vary on other distributions.

1. Clone this repo.
1. Go into the root of the project and run:

    ``` 
    vagrant up
    ```

1. After the master and 2 worker nodes are created, run the following to ssh into the master:

    ```
    vagrant ssh master
    ```

1. Run the following command to initialize Kubernetes and support joining worker nodes:

    ```
    sudo kubeadm init --apiserver-advertise-address 192.168.33.13 --pod-network-cidr=10.244.0.0/16

    # Run the commands shown

    # Copy the join command and use it in the next step
    ```

1. Join the worker nodes to the master node:

    ```
    vagrant ssh [worker1 | worker2]

    # Run the join command from the master
    ```

1. You can update the machine packages by running the following:

    ```
    sudo apt update
    sudo apt upgrade

## Credits 

This is based on a project originally created by Mehmet Ali Baykara:

https://github.com/mbaykara/k8s-cluster
