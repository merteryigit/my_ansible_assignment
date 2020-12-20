# Httpd Application Deployment

This repository consists of a set of ansible playbooks and roles which are used for deploying an httpd application on a single node or multiple nodes with a loadbalancer. 

The roles have been written in compatibility with ansible 2.6+ and CentOS 7.5

Task details can be found in role_name/tasks/main.yml file of each role.

Variables are available in role_name/vars/main.yml file of each role.

## Stack

- git
- httpd
- haproxy

## Workflow Description

The workflow consists of the following steps:

- Git installation on nodes

  This step will install git on the httpd application nodes. 
  
- Creating ssh-key and ssh configuration file for github authentication

  Ssh key name can be set by users prior to installation via 
 git/vars/main.yaml file.

- Cloning the web application repository from github

  Httpd application repository will be cloned from github. Github 
  repository url for the httpd application (avaiable at 
  git/vars/main.yaml) can be set by users prior to installation.

- Downloading and installing necessary httpd rpm package 

  httpd package will be downloaded and installed on each node. Variables can be set for this step at webapp/vars/main.yml

- Modify httpd configuration
  
  Httpd application installation path will be set in /etc/httpd/conf/httpd.conf file for running. Template for httpd.conf is available in webapp/templates/httpd.conf.j2

- Git fetch and git pull operations will be triggered for pulling code updates.

- Start httpd service

- Install and configure haproxy on loadbalancer server. Template is available for the haproxy.cfg file in haproxy/templates/haproxy.cfg.j2

## Host Configuration

Nodes that the httpd application will be deployed and the loadbalancer can be set via "hosts" file in the repository.
```
[webapp]
node1 
node2 
node3

[loadbalancer]
lb1 

```

## Usage

site.yaml file includes all written ansible roles to deploy the application and deployment can be done by running:

```
ansible-playbook site.yaml
```

## Variables

- Variables available under git/vars/main.yml

```
git_repo_url: git ssh url of the httpd application can be set
ssh_key_name: key name in order not to overlap with the default ssh key
```
- Variables available under webapp/vars/main.yml

```
webapp_listen_port: listen port for http service 
webapp_installation_path: '/opt/webapp'
rpm_url: url for downloading desired httpd rpm package
rpm_name: httpd rpm name
```

- Variables available under haproxy/vars/main.yml

```
frontend_name : name for frontend section
listen_port : listen port for frontend (80 by default)
default_backend : default backend name
backend_name :  name for backend section
balance_mode :  balance mode of choice (http by default)
lb_algorithm : loadbalancing policy of choice (roundrobin by default)
```
