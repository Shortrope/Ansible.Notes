# Install ansible

## debian.lxc, Debian8.5.lxc
- no python installed by default
- plain install gets ansible 1.7.2 and python 2.7.9
- no sudo

### Install latest ansible
currently: ansible 2.7.10, python 2.7.9

    echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> /etc/apt/sources.list
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
    apt update
    apt install -y ansible

### Optional: Install Python 3.7.3
    **Stay with Python 2.7 for now**
    apt update && apt upgrade -y
    apt install -y build-essential wget zlib1g-dev libffi-dev
    wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
    tar -xzvf Python-3.7.3.tgz
    cd Python-3.7.3
    ./configure
    make
    make install

## ubuntu-xenial.lxc
- python 3.5.2 installed by default
- plain install gets ansible 2.0.0.2 and python 2.7.12

### Install latest ansible
currently: ansible 2.7.10, python 2.7.12

    apt install -y software-properties-common
    apt-add-repository --yes --update ppa:ansible/ansible
    apt install -y ansible


## Ubuntu-16.04 KVM
- python 3.5.2 installed by default
- plain install gets ansible 2.0.0.2 and python 2.7.12

### Install latest ansible
currently: ansible 2.7.10, python 2.7.12

    sudo apt-add-repository --yes --update ppa:ansible/ansible
    sudo apt install -y ansible

## CentOS.lxc
- default: python 2.7.5
- plain install gets ansible 2.4.2.0 and python 2.7.5

### Install latest ansible
currently: ansible 2.7.10, python 2.7.5

    yum install -y epel-release  
    yum update repolist  
    yum update -y  
    yum install ansible  

## CentOS7.1 KVM
- default: python 2.7.5
- plain install gets ansible 2.4.2.0 and python 2.7.5

### Install latest ansible
currently: ansible 2.7.10, python 2.7.5

    yum install epel-release  
    yum update repolist  
    yum update  
    yum install ansible  

## Fedora
    ??
    sudo dnf install ansible

## FreeBSD
    ??
    sudo pkg install py36-ansible

## Gentoo
    ??
    echo 'app-admin/ansible' >> /etc/portage/package.accept_keywords
    emerge -av app-admin/ansible

# Ansible config file
ansible will read the .cfg file it finds first:

- ANSIBLE_CONFIG env variable
- ansible.cfg in the current working directory
- .ansible.cfg in the home directory
- /etc/ansible/ansible.cfg

## Prep Target Server
- Python 2.7 or 3.5+
- If you have SELinux enabled on remote nodes, you will also want to install libselinux-python on them before using any copy/file/template related functions in Ansible. You can use the yum module or dnf module in Ansible to install this package on remote systems that do not have it.
- you can use Ansible to install a compatible version of Python on a target host using the `raw` module
    ```
    ansible myhost --sudo -m raw -a "yum install -y python2"
    ```