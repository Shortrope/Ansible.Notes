# Configuration File

    root@ansible:/etc/ansible# ll
    total 30
    drwxr-xr-x  3 root root     5 Feb 11 03:11 ./
    drwxr-xr-x 77 root root   151 Feb 11 04:24 ../
    -rw-r--r--  1 root root   20k Feb  7 23:11 ansible.cfg
    -rw-r--r--  1 root root  1016 Feb  7 23:11 hosts
    drwxr-xr-x  2 root root     2 Feb  7 23:18 roles/

ansible will read the .cfg file it finds first:
- ANSIBLE_CONFIG,
- ansible.cfg in the current working directory
- .ansible.cfg in the home directory
- /etc/ansible/ansible.cfg

