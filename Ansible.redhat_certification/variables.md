# Variables
## Naming and quotes
- letters numbers and underscores
- Generally always use double quotes:

Examples

    debug: msg="Look! I'm using my var {{ myVar }}!"
    name: "{{package}}"

## Places to define variable
Roles, Blocks and inventories

### 1. vars
    vars: 
      userFile: /home/user/vars/userList

### 2. vars_files
_users.lst_

    staff:
      - joe
      - josie
      - bob
      - beth
    faculty:
      - matt
      - mary
      - martha
    other:
      - will
      - wendy

_myplaybook.yml_

    vars_files: 
      - /home/user/vars/users.lst


### 3. vars_prompt
Can prompt for vars during Playbook execution!

### 4. via the command line
Using the `-e` option 
- extra vars  
- @ for extra vars in a file

Example:

    ansible-playbook play.yml -e '{"myVar":"myValue","anotherVar":"anotherValue"}'
    ansible-playbook play.yml -e "@users.lst"

users.lst is a file

## Dictionary variables

    employee:
      name: bob
      id: 43
    
Access the vars

    employee['name']
    employee.name

Bracket sysntax is safer

## Magic vars and filters
Ansible defines several special vars known as magic vars
- hostvars: Look at facts about other hosts in the inventory
- groups: provides group inventory info
- group_names:
- inventory_hostname

### `hostvars`
facts about other hosts in the inventory

    "{{ hostvars['node1']['ansible_distribution'] }}"

### `groups`
Provides inventory information

    "{{ groups['webservers'] }}"

### `Jinja2 filters
http://jinja.pocoo.org/docs/2.10/templates/#builtin-filters  
Manipulating text format  
Uses the pipe function - |  

    "{{ groups['webservers']|join(' ') }}"

## Facts
Info descovered about the target system  
2 Ways Facts are collected

- ad-hoc command `ansible all -m setup -a "fileter=*ipv4*"`
- can use filters w regex in the ad-hoc mode
- Gathered by default when a playbook is run
- Conditionals can use Facts to make decision


Collected Facts can be accessed through variables  

- {{ ansible_default_ipv4.address }}

Examples:

    ansible all -m setup | less
    ansible all -m setup -a "fileter=*ipv4*"
    ansible all -m setup -a "fileter=*dist*"


## Facts.d - custom facts
Can create custom facts for a server
- /etc/ansible/facts.d/*.fact
    - data.fact
    - prefs.fact
    - software_inv.fact
- File formats:
    - INI
    - JSON
    - an executable that returns JSON

Facts File example:

    [location]
    type=physical
    datacenter=SanDiego

Retrieve the custom facts

    ansible myhost -m setup -a "fileter=ansible_local"

## Examples
Write the list of Labservers in the inventory to another file on the local machine  

    ---
    - hosts: local
      vars:
        inv_file: /home/user/vars/inv.txt
      tasks:
      - name: create group inventory file
        file:
          path: "{{inv_file}}"
          state: touch
      - name: generate inventory
        lineinfile:
          path: "{{inv_file}}"
          line: "{{ groups['labserver'] }}"
          line: "{{ groups['labserver']|join(' ') }}"













------

Can have a variables section in a playbook  

    vars:
      author_name: Mak

or with yaml

    foo:
      field1: one
      field2: two

Access the vars

    foo['field1']       # prefered
    foo.field2

Apply vars directly in a playbook  

_appserver.yml_

    - hosts: appserver
      vars:
        control_server: localhost
        web_root: /var/www/html/
      tasks:
      - name: Install tmux on app servers
        apt: pkg=tmux state=present update_cache=true

## Variables file

_vars.yml_

    control_server: localhost
    web_root: /var/www/html/


_appserver.yml_

    - hosts: appserver
      vars_files:
      - vars.yml
      tasks:
      - name: Install tmux on app servers
        apt: pkg=tmux state=present update_cache=true