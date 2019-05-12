# Ansible Commands

    ansible all --list-hosts
    ansible <host-group> -m ping                # test ssh connectivity to all hosts in the group
    ansible <host-group> -i myhostfile -m ping  # -i specity hosts (inventory) file
                                                # -m use module

    ansible <host-group> -b -m shell -a 'apt install tmux'
                                                # -b become user. default method is sudo
                                                # -m shell : run a shell command
                                                # -a ??

    ansible <host-group> -m setup               # gets 'facts' 
    ansible <host-group> -m setup --tree ./facts        # saves 'facts' in a file
    ansible <host-group> -m setup -a 'filter=*ipv4*'    # filter 'facts' 
    ansible <host-group> -m setup | grep ipv4           # compare to filter 'facts' 
    ansible <host-group> -m setup -a 'filter=ansible_distribution' 
    ansible <host-group> -m setup -a 'filter=ansible_distribution_version' 
