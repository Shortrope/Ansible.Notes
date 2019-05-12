# Inventory File
An Inventory is a list of hosts that Ansible manages  
- Default location /etc/antsible/hosts
- The location can be set in ansible.cfg
- The Inventory can be a File or Directory containing inventory files
- Can be specified on the CLI w -i option
- The file can be in Yaml or Ini format

The inventory file can contain
- hosts
- patterns
- groups
- variables - not recommended to put in the inventory files, rather a separate file


INI version
- its ok to put hosts in more than 1 group
- add non-standard ssh port number w a colon :1964

Example:

    mail.example.com
    otherssh.port.com:1964
    www[01:20].example.com      # can have leading zeros
    db-[a:f].example.com
    
    [webservers]
    foo.example.com
    bar.example.com     http_port:8080

    [dbservers]
    one.example.com
    two.example.com
    three.example.com

## Dynamic Inventory
- Executable script - bash, python...
- Good for resources that change suddenly/dynamically - like cloud resources
- Script returns the inventory in JSON format to stdout
- Must respond to two possible parameters
    - --list
    - --host [hostname]

Example

    ansible all -i dynamic.py -m ping


# Groups
There are two default groups
- all
- ungrouped

## Child Groups

    [atlanta]
    host1
    host2

    [raleigh]
    host2
    host3

    [southeast:children]
    atlanta
    raleigh

    [southeast:vars]
    some_server=foo.southeast.example.com
    halon_system_timeout=30
    self_destruct_countdown=60
    escape_pods=2

    [usa:children]
    northwest
    southwest
    northeast
    southeast

List hosts

    ansible all --list-hosts

## Variables
Best practice to use `host_vars` or `group_vars`

    localhost            ansible_connection=local
    other1.example.com   ansible_connection=ssh     ansible_user=mario
    other2.example.com   ansible_connection=ssh     ansible_user=ansible 
    jumper ansible_port=5555 ansible_host=192.0.2.50
    ubuk1 ansible_host=10.1.1.121 ansible_user=ubuntu
    bar.example.com     http_port:8080

The order/precedence is (from lowest to highest):

    'all' group         # it is the ‘parent’ of all other groups
    parent group
    child group
    host

## `group_vars` and `host_vars`
**This is the 'Best Practice' to separate the variables from the inventory file**  

These variable files are located either:
- relative to the inventory file  
- in the playbook directory         # takes precedence

For the [webservers] group

    /etc/ansible/hosts
    /etc/ansible/group_vars/webservers.yml      # can optionally end in '.yaml', or '.json'

For the host 'web1'

    /etc/ansible/hosts
    /etc/ansible/host_vars/web1.yml

Or create directories named after the groups or hosts

    /etc/ansible/group_vars/webservers/db_settings.yml
    /etc/ansible/group_vars/webservers/cluster_settings.yml

Contents:

    ---
    ansible_python_interpreter: /usr/bin/python3
    ansible_user: ansible
    ntp_server: ntp1.example.com


### Group Variables
Variables common to entire group

    [atlanta]
    host1
    host2

    [atlanta:vars]
    ntp_server=ntp.atlanta.example.com
    proxy=proxy.atlanta.example.com

## aliases w variables

    jumper ansible_port=5555 ansible_host=192.0.2.50
