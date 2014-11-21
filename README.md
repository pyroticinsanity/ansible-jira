# Ansible deployment for JIRA

## 1. QuickStart

```
# Deploy to a remote
ansible-playbook jira.yml 

```


## Table of contents

- [1. QuickStart](#1-quickstart)
- [2. Overview](#2-overview)
- [3. Requirements](#3-requirements)
- [4. Usage](#4-usage)
  - [4.1 Playbook Arguments](#41-playbook-arguments)
- [5. After Install](#5-after-install)


## 2. Overview

An Ansible playbook to automate the installation of Atlassian JIRA on a TurnKey Linux Debian system.

The Task performed by the playbook are:

	- install or check if java-1.7.0-openjdk-devel is installed
        - install MySQL
	- create a group for jira
	- create a user that belong to the group created before
	- Download the standalone tar file given a version
	- extract the tar file
	- create a symlink to a "current" directory for the extracted dir
	- set the jira-home dir in the jira-application.properties file
	- copy an init script or a service file depending on the EL major release 
	- sets up the MySQL database and user as per the JIRA guidelines
	- sets up a Java KeyStore for the SSL certificate
	- modifies JIRA to use SSL

## 3. Requirements

Ansible to be installed in the local machine.

```
git clone git://github.com/ansible/ansible.git --recursive
cd ./ansible
source ./hacking/env-setup
```

## 4. Usage

1. `git clone http://github.com/pyroticinsanity/ansible-jira` 
2. modify the jira/vars/main.yml to have your desired values
3. run the Ansible playbook, the fastest way is:

```
ansible-playbook jira.yml 
```

### 4.1 Playbook Arguments

- jira_user_group: group that jira files are going to belong to
- jira_user_group_gid: group id 
- jira_user: user that jira files are going to belong to
- jira_user_uid: user id
- jira_user_home_dir: path to where the sources are going to be, Default: /opt
- jira_home: jira-home path, if it's the first time installation use whereever you like, otherwise use your current
- jira_version: the version check latest in atlassian site
- jira_version_file_sha256sum: you should have this 
- jira_download_link: link to download the .tar.gz file (note: it will be concatenated with the version var)

- mysql_root_password: configures MySql with the given root password 
- mysql_jira_db: the MySQL database to store JIRA's data in
- mysql_jira_user: the MySQL user that jira will use
- mysql_jira_password: the MySQL password that jira will use

- ssl_alias: the SSL alias to use for the keystore
- ssl_dname: the domain name to create the SSL certificate with. i.e. CN=jira,OU=SW,O=O,L=Edmontom,S=Alberta,C=CA
- ssl_keypass: the keypass for the keystore
ssl_storepass: the storepass for the keystore

```

**TODO** use the handler after install to run the application

### 5. After install

You can just ssh to the server(s) (more than one if you are using jira datacenter) and:

for releases version 6 or less [works with 7 as well]
```
service jira start
```
Enjoy
