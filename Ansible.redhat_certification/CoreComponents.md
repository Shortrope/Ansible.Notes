# Core Components
## Inventories
List and groups of hosts  
Can use **.ini** or **yaml** format

    mail.example.com

    [webservers]
    raleigh.example.com
    dallas.example.com

    [dbservers]
    db[1:4].example.com

VS

    all:
      hosts:
        mail.example.com
      children:
        webservers:
          hosts:
            raleigh.example.com
            dallas.example.com
        dbservers:
          hosts:
            db[1:4].example.com
            


## Modules
- Tools for a particular task  
- Usually take parameters  
- They return JSON  
- Can be run from the command line or from a Playbook  
- Lots of 'em


## Variables
- letters numbers and underscores  
- start w a letter
- Can be scoped by group, host or playbook
- Typically used for configuration values and parameters
- Can store the return value of executed commands
- Can be used as dictionaries
- Many predefined variables

## Facts
- Provide information about a given host
- Facts are discovered automatically when reaching out to a host
- Can be disabled
- May be cached between playbook executions

## Plays
- The goal of a `play` is to map a group of hosts to some well-defined roles
- A play may use one or more modules to achieve a desired state on a group of hosts

## Playbooks
- A playbook is a series of plays
- Can deploy configuration to multipule servers to support a new application

## Configuration files
ansible will read the .cfg file it finds first:
- ANSIBLE_CONFIG,
- ansible.cfg in the current working directory
- .ansible.cfg in the home directory
- /etc/ansible/ansible.cfg

Any configuration stored in an environment variable overrides the .cfg file  
Commonly used setting  
- ansible_managed
- forks
- inventory


