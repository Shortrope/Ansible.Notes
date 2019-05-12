# Prepare New Host

1. Get ssh fingerprint
1. Copy ssh key
1. update and upgrade
1. Install python
1. Create ansible user
1. Give ansible user sudo permissions
1. Set hostname

## answer 'yes'
./storm-install/storm_install.sh all `hostname` $INSTALLDIR  <<-EOF
yes
EOF

echo yes | ./storm-install/storm_install.sh all `hostname` $INSTALLDIR

( echo yes ; echo no; echo yes ) | script.sh


## get ssh fingerprint

    tasks:
    - name: get ssh fingerprint 1
      shell: ssh-keyscan -H 10.1.1.10

    - name: get ssh fingerprint 2
      known_hosts:
        path: ~/.ssh/known_hosts
        name: 10.1.1.10

## Install
---
- hosts: newbies
  gather_facts: False
  remote_user: root
#  remote_user: ubuntu
#  become: yes
#  become_user: root
#  become_method: sudo

  tasks:
    - name: Install Python
      raw: test -e /usr/bin/python || (apt update && apt install -y python-minimal --no-install-recommends)

    - name: Fancy way of doing authorized_keys
      authorized_key: user=root
                      exclusive=no
                      key="{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

    #- name: Create /root/.ssh
    #  file: path=/root/.ssh state=directory mode=0700

    #- name: Create authorized_keys
    #  lineinfile: dest=/root/.ssh/authorized_keys line="{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
