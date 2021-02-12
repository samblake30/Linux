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
   * ***Step1:-*** _Set Hostname with its IP address_
      * _Chanage the Hostname and add the Hostname of all Hosts with those IP address in the ```/etc/hosts``` file (Consider all Hosts or VM as Nodes) as below_
      ```bash
         hostnamectl set-hostname k8s-app01
         
         cat /etc/hosts
         192.168.2.49 k8s-app01
         192.168.3.50 k8s-app02
         192.168.5.51 k8s-app03
      ```
   * ***Step2:-*** _Update the OS packages_
      ```bash
      yum update -y
      ```
   * ***Step3:-*** _Disable the SElinux or change the mode to permissive this will help all containers access the host filesystem_
      ```bash
      setenforce 0
      sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
      ```
   * ***Step4:-*** _Allow the below ports in the firewall or in the security group of the instance_
      * ***Master Node***
         Service      |  Ports   | Protocol
         -----------      | --------------------   | --------------------
         _Kube-APIServer_                           :point_right:  | ***6443***        | ***TCP***
         _ETCD_                                     :point_right:  | ***2379 - 2380*** | ***TCP***
         _Kubelet HealthCheck Port_                 :point_right:  | ***10250***       | ***TCP***
         _Kube-Scheduler_                           :point_right:  | ***10251***       | ***TCP***
         _Kube-Controller-manager_                  :point_right:  | ***10252***       | ***TCP***
         _Read-Only Kubelet-API_                    :point_right:  | ***10255***       | ***TCP***
       
       * ***Worker Node***
          Service     | Ports   | Protocol
          ----------      | --------------------   | ---------------------
          _Kubelet-API_                             :point_right:  | ***10250***         | ***TCP***
          _Read-Only Kubelet-API_                   :point_right:  | ***10255***         | ***TCP***
          _NodePort Services_                       :point_right:  | ***30000 - 32767*** | ***TCP***
   * ***Step5:-*** _To Install Docker and Kubernetes in nodes, need to configure docker and Kubernetes repositories_
      * _Kubernetes Repo_
       ```bash
       ~ cat << EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
       > [kubernetes]
       > name=Kubernetes
       > baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
       > enabled=1
       > gpgcheck=1
       > repo_gpgcheck=1
       > gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
                https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
       > EOF
       ~ 
       ```
       * _Docker Repo configuration_
       ```bash
       ~ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
       ```
    
   * ***Step6:-*** _Install Kubernetes and Docker components_
      ```bash
      #On Master do below Commands
      ~ yum install kubelet kubeadm kubectl docker -y
      
      #On Worker
      ~ yum install kubelet docker -y
      ```
   * ***Step7:-*** _Enable and Start Docker and Kubernetes Services_
      ```bash
      ~ systemctl enable --now docker
      ~ systemctl enable --now kubelet
      ```
   * ***Step8:-*** _Run the Cluster configuration on the Master/Control Plane node._
      ```bash
      ~ kubeadm init --pod-network-cidr=10.244.0.0/16
      ```
      * _You can see the output from above command. After the successful start of kubadm master, we need to run the shown command from the non-root or root user then only a user can control the kubectl commands._
      ```bash
      ~ mkdir -p $HOME/.kube
      ~ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      ~ sudo chown $(id -u):$(id -g) $HOME/.kube/config
      ```
      
      

      

