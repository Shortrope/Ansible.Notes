# Inventory Management
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

## File format

## Ini

    mail.example.com ansible_port=5556 ansible_host=192.168.0.10

    [webservers]
    httpd1.example.com
    httpd2.example.com

    [webserver:vars]
    http_port=8080

    [labservers]
    lab[01:99].example.com

### Yaml

    ---
    all:
      hosts:
        mail.example.com:
          ansible_port: 5556
          ansible_host: 192.168.0.10
      children:
        webservers:
          hosts:
            httpd1.example.com
            httpd2.example.com
          vars:
            http_port: 8080
        labservers:
          hosts:
            lab[01:99}.example.com]


## Static Inventory
- INI or YAML format
- Maintained manually
- Easy to manage for static hosts

## Dynamic Inventory
- Executable script - bash, python...
- Good for resources that change suddenly/dynamically - like cloud resources
- Script returns the inventory in JSON format to stdout
- Must respond to two possible parameters
    - --list
    - --host [hostname]

Example

    ansible all -i dynamic.py -m ping


## Variable w inventories
- Not recommended to put variables in the inventory files, rather a separate file
- Use a separate Yaml File for vars relative to the inventory file
    - ./group_vars/
    - ./host_vars/
    - Files are named by host or group

Example directory structure

    ./inventory.ini
    ./group_vars/webservers.yml
    ./host_vars/mail.example.com.yaml

_webservers.yml_

    http_port: 8080
    logs: /var/log/httpd.log

_mail.example.com.yaml_

    ansible_port: 5556
    ansible_host: 192.168.0.10

Example using a variable

    ansible webservers -i inventory.ini -b -a "tail {{logs}}"
