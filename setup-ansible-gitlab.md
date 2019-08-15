# How to install Ansible on Ubuntu and Centos

## Install Ansible on Ubuntu

**Update Ubuntu repo cache , install ansible repo and install the packages**

```
sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

**How to upgrade Ubuntu to latest release**

```
sudo apt-get install update-manager-core
cat /etc/update-manager/release-upgrades | grep Prompt (make sure it is set to normal (no lts))
sudo do-release-upgrade
```
Upgrade documentation can be found [here](https://websiteforstudents.com/how-to-upgrade-from-ubuntu-17-10-artful-aardvark-to-ubuntu-18-04-lts-beta/)

## Install Ansible on Centos 

Update Centos repo cache , install ansible repo and install the packages
```
sudo yum update
sudo yum upgrade
sudo yum install ansible tree
ansible --version
```
Add Firewall rules for http traffic
```
sudo iptables -L
sudo setstatus
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
sudo iptables -L | grep http
sudo sshd --permanent --add-service=http
sudo systemctl reload sshd
sudo iptables -L | grep ssh
```
Create ansible files and directory structure
```
mkdir -p ansible/{hosts,groups,roles,logs}
cd ansible
mkdir hosts/host_var
mkdir groups/group_var
touch logs/ansible.log
touch hosts/hosts.yml
```
Configure Ansible main configuration file /etc/ansible/ansible.cfg
```
<span style="background-color:grey">
[defaults]
; reference: http://docs.ansible.com/intro_configuration.html
ansible_managed               = Ansible managed
ask_pass                      = True
command_warnings              = True
deprecation_warnings          = True
display_skipped_hosts         = True
error_on_undefined_vars       = True
forks                         = 10
gathering                     = implicit
http_user_agent               = ansible-agent
host_key_checking             = False
log_path                      = /devops/ansible/logs/ansible.log
nocows                        = 1
hosts                         = *
remote_tmp                    = $HOME/.ansible/tmp
roles_path                    = /devops/ansible/roles
system_warnings               = True
timeout                       = 30
transport                     = ssh
allow_world_readable_tmpfiles = True
; library/Plugins
library                       = ~/.ansible/lib/plugins/modules/:/usr/lib/python2.7/site-packages/modules/
action_plugins                = ~/.ansible/lib/plugins/action/:/usr/lib/python2.7/site-packages/ansible/plugins/action/
callback_plugins              = ~/.ansible/lib/plugins/callback/:/usr/lib/python2.7/site-packages/ansible/plugins/callback/
connection_plugins            = ~/.ansible/lib/plugins/connection/:/usr/lib/python2.7/site-packages/ansible/plugins/connection/
filter_plugins                = ~/.ansible/lib/plugins/filter/:/usr/lib/python2.7/site-packages/ansible/plugins/filter/
lookup_plugins                = ~/.ansible/lib/plugins/lookup/:/usr/lib/python2.7/site-packages/ansible/plugins/lookup/
vars_plugins                  = ~/.ansible/lib/plugins/vars/:/usr/lib/python2.7/site-packages/ansible/plugins/vars

; retry files
retry_files_enabled           = True
retry_files_save_path         = ~/

[inventory]
inventory                     = /devos/ansible/hosts/hosts.yml
[privilege_escalation]
become                        = False
become_method                 = sudo
become_ask_pass               = False
[paramiko_connection]
[ssh_connection]
pipelining                    = True
[persistent_connection]
[accelerate]
[selinux]
special_context_filesystems   = nfs,vboxsf,fuse,ramfs
[colors]
[diff]
</span>
```

`ansible-config view`

Add RH repo and python manager
```
sudo yum install -y epel-release && sudo yum update && sudo yum install -y python-pip
```
# How to install Gitlab on Ubuntu and Centos

## Install GitLab on Centos

Install additional packages and install GitLab though https
```
sudo yum install -y curl policycoreutils-python openssh-server
sudo yum install postfix
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.edotcom.be" yum install -y gitlab-ee
```
setup your password and login back as root

login to http://gitlab.edotcom.be/ and create a new account
then you can login to http://gitlab.edotcom.be/
```
ssh-keygen -t rsa -b 4096 -C "<your-full-name>"
```
## Install GitLab on Ubuntu

Install additional packages and install GitLab though https
```
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates
sudo apt-get install -y postfix
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.edotcom.be" apt-get install gitlab-ee
```
setup your password and login back as root

##Installing Basic Git on Ubuntu
```
sudo apt install git-all
```
# Maintenance commands for Gitlab

[Reference](https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab)

Main configuration file is located here at /etc/gitlab/gitlab.rb
```
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```
## Ansible Best Practices

Ansible Directory structure
```
sudo mkdir -p /devops/ansible/{hosts,groups}
sudo mkdir -p /devops/ansible/hosts/host_vars
sudo mkdir -p /devops/ansible/groups/group_vars
sudo mkdir -p /devops/ansible/{etc,roles,templates,libraries,modules,logs}
```
