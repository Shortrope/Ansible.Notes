# Roles
Roles provide a way of automatically loading var_files, tasks, and handlers based on a known file structure.

Roles also make sharing of configuration templates easier  
At least one of the directories must exist and contain a 'main.yml'.

ansible  
|--ansible.cfg  
|--hosts  
|--facts.d/  
|--roles/  
    |--sample_role
        |--tasks/
            |--main.yml
        |--handlers/
        |--files/
        |--templates/
        |--vars/
        |--defaults/
        |--meta/


vars/main.yml
- has the highest level of precedence
- Will override inventory variables as well
- only vars passed via the CLI have higher precedence

defaults/main.yml
- lowest level of precedence
- privides a value to a var if no other value is given

Vars can be accessed across roles  
Best practice: namespace your vars w/in a role to avoid conflics

    common_ip
    db_user

handlers/main.yml
- tasks flagged to run using the notify keyword
- notify only calls the handler if the task makes changes
- handler will only be triggered once even if triggered by multiple tasks

files/*.raw.files
templates/*.j2
- These can be refereced w/out a path

## Example playbook

    ---
    - hosts: local
      become: yes
      roles:
        - apache
        - mysql

Alternate use: allows use of conditionals and tags

    ---
    - hosts: webservers
      tasks:
      - include_role:
        name: apache
        tags:
        - RD_HTTPD
        when: "ansible_os_family=='RedHat'"

## meta
the meta directory defines meta data for the role.  

- Dependencies
- role level configs like 'allow_duplicates: true'

allows a role to be applied more than once

## Dependencies
in _meta/main.yml_

    ---
    dependencies:
      - role: common
        vars:
          some_parameter: 3
      - role: apache
        vars:
          apache_port: 80


## roles/baseline/tasks
tasks/  
|--main.yml  
|--create_motd.yml  
|--deploy_nagios.yml  
|--edit_hosts.yml  

_main.yml_

    ---
    - name: create motd
      import_tasks: create_motd.yml
    - name: deploy nagios client
      import_tasks: deploy_nagios.yml
    - name: edit hosts file
      import_tasks: edit_hosts.yml