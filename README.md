# DeployerVPN
This repository provides a set of Ansible Playbook for automatic deployment of OpenVPN on servers running Ubuntu 20.04, support for other OS versions such as Ubuntu 18.04 or Debian is planned in the future. VPN can be deployed in Single, Double and Triple nodes.

## Minimum server requirements

Server with 1 CPU, 1024 MB RAM, 5 Gb free HDD or SDD,  running Ubuntu 20.04

## Clients tested 

* Android App OpenVPN Connect 3.2.7
* iPhone App OpenVPN Connect 3.2.3    
* OS X Big Sur with Tunnelblick 3.8.7a using openvpn-2.5.4
* Windows OpenVPN GUI v11.26.0.0

## Quick Start

To use it, you need to install Ansible version at least 2.21 installation instructions on [your system here](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)

Next, download this repository

      git clone https://github.com/iamDeployer/DeployerVPN.git

Go to the repository folder and launch Ansible Playbook
For convenience, you can edit the inventory file hosts.txt or pass parameters via –extra-vars

## Single Node

      ansible-playbook -i ./hosts.txt -l test playbooks/vpn_standalone.yml –extra-vars “clientname=test profile_path=~/openvpn_profiles”  

Required variables:
* `clientname: test` - specify the name of the client, for example test

## Double Node 

!!!The sequence of execution is important!!!

      ansible-playbook -i ./hosts.txt -l test playbooks/double_vpn_exitnode.yml –extra-vars “client_profile=vpn_test_hop2”    

Required variables:
* `client_profile: vpn_{client_name}_hop2` - specify the profile name, change only the client name example vpn_test_hop2

      ansible-playbook -i ./hosts.txt -l test playbooks/double_vpn_enternode.yml –extra-vars “clientname=test nextsrv_client_profile=vpn_test_hop2 profile_path=~/openvpn_profiles”   

Required variables:
* `clientname: test` - specify the name of the client, for example test
* `nextsrv_client_profile: vpn_{client_name}_hop2` - specify the profile name, example vpn_test_hop 2

Optional variables:
* `profile_path: /root/openvpn_profiles` - the path where to save the client connection file

## Triple Node

!!!The sequence of execution is important!!!

      ansible-playbook -i ./hosts.txt -l test playbooks/triple_vpn_exitnode.yml–extra-vars “client_profile=vpn_test_hop3”   

Required variables:
* `client_profile: vpn_{client_name}_shop3` - specify the profile name, change only the client name example vpn_test_hop3

      ansible-playbook -i ./hosts.txt -l test playbooks/triple_vpn_middlenode.yml –extra-vars “client_profile=vpn_test_hop2 nextsrv_client_profile=vpn_test_hop3 ”   

Required variables:
* `client_profile: vpn_{client_name}_hop2` - specify the profile name, change only the client name example vpn_test_hop2
* `nextsrv_client_profile: vpn_{client_name}_hop3` - specify the profile name, change only the client name example vpn_test_hop3

      ansible-playbook -i ./hosts.txt -l test playbooks/triple_vpn_enternode.yml  –extra-vars “clientname=test nextsrv_client_profile=vpn_test_hop2 profile_path=~/openvpn_profiles”   

Required variables:
* `clientname: test` - specify the name of the client, for example test
* `nextsrv_client_profile: vpn_{client_name}_hop2` - specify the profile name, example vpn_test_hop2

Optional variables:
* `profile_path: /root/openvpn_profiles` - the path where to save the client connection file
