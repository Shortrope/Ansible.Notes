# Playbooks
## How to work with common modules
- Files
    - copy
    - archive, unarchive
    - get_url
- User, Group
- yum, service
- ping

### Lininfile module
### htpasswd
### Shell and command modules
### Script module
### Debug module

---

## Configure a specified state
- Plays **map** a group of hosts to _well-defined_ **roles**  
- Playbooks orchestrate complex ativities - system or application deployment

Example:

    ---
    - hosts: webservers
      become: yes           # config system user to sudo su w/out a password

      tasks:
      - name: ensure httpd is at the latest version
        yum: name=httpd state=latest
      - name: write the httpd config file
        template: src=/srv/httpd.j2 dest=/etc/httpd.conf
      - name: start and enable httpd
        service:
          name: httpd
          state: started
          enabled: yes


### Retry
A file is created named `<playbook>.retry`  
The _.retry_ file allows you to rerun the playbook only on the failed machines  

### Limit
- Execute against one host  
- Take the `limit` off and run against everything  
- Watch out for indentation (spaces)

Example:

    ansible-playbook demo.yml --limit web1.example.com


## Use vars to retrieve the results of commands
Use `register:` in the playbook  

    ---
    - hosts: web1.example.com
      tasks:
      - name: copy file
        copy:
          src: testfile
          dest: /home/user/testregister
          mode: 0400
        register: var
      - name: output debug info
        debug: msg="debug info is {{ var }}"
      - name: save var in file
        lineinfile:
          path: /tmp/newfile
          line: "The uid is {{ var.uid }}"


## Use conditionals for play execution

## Error handling

## Tags




Install a package

    ansible apacheweb -b -m yum -a 'pkg=lynx state=installed update-cache=true'

Playbook

_appserver.yml_

    - hosts: appserver
      tasks:
      - name: Install tmux on app servers
        apt: pkg=tmux state=present update_cache=true