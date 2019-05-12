# System Facts

    ansible <host-group> -m setup               # gets 'facts' 
    ansible <host-group> -m setup --tree ./facts        # saves 'facts' in a file
    ansible <host-group> -m setup -a 'filter=*ipv4*'    # filter 'facts' 
    ansible <host-group> -m setup | grep ipv4           # compare to filter 'facts' 

Common facts:

    ansible <host-group> -m setup -a 'filter=ansible_architecture' 
    ansible <host-group> -m setup -a 'filter=ansible_distribution' 
    ansible <host-group> -m setup -a 'filter=ansible_distribution_version' 
    ansible <host-group> -m setup -a 'filter=ansible_domain'
    ansible <host-group> -m setup -a 'filter=ansible_fqdn'
    ansible <host-group> -m setup -a 'filter=ansible_interfaces'
    ansible <host-group> -m setup -a 'filter=ansible_kernel'
    ansible <host-group> -m setup -a 'filter=ansible_memtotal_mb'
    ansible <host-group> -m setup -a 'filter=ansible_processor*'
    ansible <host-group> -m setup -a 'filter=ansible_virtualization*'
