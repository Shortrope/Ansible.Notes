# hosts file

_/etc/ansible/hosts_

    [local]
    # localhost
    127.0.0.1

    [apacheweb]
    # ansible2
    192.168.1.11

    [appserver]
    # ansible3
    192.168.1.12

## List hosts

    ansible all --list-hosts

## Check connectivity to hosts

    ansible apacheweb -m ping
    ansible myhosts -i ~/playbooks/hosts -m ping    # -i  inventory file