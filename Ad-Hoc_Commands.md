# Ad-Hoc Ansible Commands
Perform a single command on a single or group of hosts
Just one-liners

## Use Cases for Ad-Hoc

Operational Commands
- Checking log contents
- Daemon control
- Process management

Informational commands
- Check installed software
- Check system properties
- Gather system performance info(cpu, disk space, mem use)

Research
- Work w unfamiliar modules on test systems
- Practice for playbook engineering

## Ad-hoc vs Playbook

### Ad-hoc mode
Command: `ansible`


    ansible httpd -i ansibile/inv.ini -b -m yum -a "name=httpd state=latest" -f 100

        -i  inventory file
        -b  become user.  default is root
        -m  module
        -a  arguments
        -f  fork - for parallelism. default will act on 5 systems simultaneously

        ansible <host> -a "run-checks"    # if no module, like running a shell command 

## Modules
### Ping
- Validate server is up and reachable
- No required parameters

### Setup
- Gather ansible facts
- No required parameters

### Yum
- Use yum package manager
- Required: 'name' and 'state'
    - state: installed, present, latest, absent, removed

### Service
- Control Daemons
- Required:
    - name: service name
    - state: start, stopped, restarted, reloaded

### User
- Manipulate sytem users
- Required: name

### Copy
- Copy files
- Required: 'src' and 'dest'

### File
- Work with files
- Required: path

### Git
- Interact w git repositories
- Required: 'repo' and 'dest'

## Example Commands
### Install/Remove package
    ansible centos -i hosts.ini -m yum -a "name=elinks state=latest"  
    ansible centos -i hosts.ini -m yum -a "name=elinks state=absent"
    ansible all -b -m service -a "name=apache2 state=started"

### File 
    ansible centos -i hosts.ini -m file -a "path=/root/.bashrc"         # does file exist
    ansible centos -i hosts.ini -m file -a "path=/root/ansibleFILE state=touch"
    ansible all -m copy -a "src=~/.vimrc dest=/root/.vimrc"

### User
    ansible centos -i hosts.ini -m user -a "name=sam"
    ansible centos -i hosts.ini -m user -a "name=sam groups=wheel append=yes"
