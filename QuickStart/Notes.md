# Ansible Notes
- Can execute tasks on remote or local systems
- Operates over SSH
- ad-hoc commands

ad-hoc commands  

    ansible localhost -m setup

## Install 
## config file
## Inventory file
### host patterns
### host variable, alias

## SSH 
- ansible -k --ask-pass         # prompt for ssh pw. causes manual intervention
- ssh-copy-id
- ssh-keyscan
- Ansible best implemented using a common user across all controlled systems.
- /etc/sudoers w/out password - use visudo
    - ansible ALL=(ALL) NOPASSWD: ALL
- ansible -K --ask-sudo-pass    # prompt for sudo pw. again causes manual intervention

## Documentation
- [docs.ansible.com](https://docs.ansible.com)
    - Module Index
- `ansible-doc` module_name         # cli documentation
    ansible-doc -s lineinfile       # -s summary
    ansible-doc -l                  # -l list installed modules

# ad-hoc commands
Perform a single command on a single or group of hosts  

