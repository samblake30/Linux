# _Kubernetes Setup ![kubernetes](https://img.shields.io/badge/%E2%9A%A1-Kubernetes-orange) ![Docker](https://img.shields.io/badge/%E2%9A%A1-Docker-yellow) ![Container](https://img.shields.io/badge/%E2%9A%A1-Containerd-blue)_

## _General Description_
 * _Kubernetes is simply to automate our application infrastructure and make it easy to manage.So if we're using containers and we have this application infrastructure that's built on containers, Kubernetes makes it much easier to automate and manage that infrastructure. what containers really are because Kubernetes is all about managing containers. So at a high level, containers are simply something that wraps software and independent and portable packages. And when we wrap software in these independent in portable packages, what we're doing is we're making it easy to run that software consistently in a variety of environments and ensure that it runs the same way regardless of what kind of environment it's in. Let's Start with the basic Setup of Kubernetes cluster using the ```Kubeadm``` utility._
 
## _Set-Up Architecture_

   <p align="center">
      <img width="500" height="450" src="https://github.com/samblake30/Linux/blob/main/Kubernetes/images/architecture.png">
   </p>

## _Pre-Requisite_

* _3 Servers with RHEL outof which 1 is going to be Master/Control-Plane and other 2 are going to be the Worker Nodes just as explained in above architecture._
* _OS Red Hat Enterprise Linux 8.3 (Ootpa)  4 cores and min 4 GB RAM

* _Let’s start how to install and configure Kubernetes in Redhat Enterprise Linux._
***_Note_:-*** _You’ll also need a user account with sudo privileges and access to the root user account._

### _Steps for Kubernetes Cluster Configuration:-_
   * ***Steps1:-*** _Set Hostname with its IP address_
      * _Chanage the Hostname and add the Hostname of all Hosts with those IP address in the ```/etc/hosts``` file (Consider all Hosts or VM as Nodes) as below_
      ```bash
         hostnamectl set-hostname k8s-app01
         
         cat /etc/hosts
         192.168.2.49 k8s-app01
         192.168.3.50 k8s-app02
         192.168.5.51 k8s-app03
      ```

      

