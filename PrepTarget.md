# Prep Target
- Python 2.7 or 3.5+
- If you have SELinux enabled on remote nodes, you will also want to install libselinux-python on them before using any copy/file/template related functions in Ansible. You can use the yum module or dnf module in Ansible to install this package on remote systems that do not have it.
- you can use Ansible to install a compatible version of Python on a target host using the `raw` module
    ```
    ansible myhost --sudo -m raw -a "yum install -y python2"
    ```

---

- Get ssh key - ssh-keyscan
- Install python if needed (minimum: 2.7 or 3.5+ )
- ssh-copy-id to each server both by IP and name  (try the `expect` module)
- ssh-copy-id to each server both by IP and name for the ansible user
- Create an ansible user on each target that will run ansible tasks  

Example inventory file

    [apt]
    deb1 ansible_host=10.1.1.30
    ubu1 ansible_host=10.1.1.31
    ubuk1 ansible_host=10.1.1.32 ansible_user=ubuntu

Example tasks

    ---
    - hosts: prep:newbies
      gather_facts: False

    # Ubuntu16.04 KVM
      remote_user: ubuntu
      become: yes
      become_user: root

    tasks:

    # Get ssh key
        - name: Get target ssh key
          delegate_to: localhost
          blockinfile:
            path: ~/.ssh/known_hosts
            create: yes
            block: "{{ lookup('pipe', 'ssh-keyscan -H ' + ansible_host) }}"
            marker: "# {mark} ANSIBLE MANAGED BLOCK: {{ inventory_hostname }}"

    # Python install

        - name: Install Python
          raw: test -e /usr/bin/python || (apt update && apt-get install -y python --no-install-recommends)
          raw: test -e /usr/bin/python || (apt update && apt-get install -y python-minimal)
          raw: test -e /usr/bin/python || yum install -y python
          raw: test -e /usr/bin/python || yum install -y libselinux-python
          raw: test -e /usr/bin/python || dnf install -y python

    # Copy ssh key to authorized_keys file on target
        - name: Ansible way of doing authorized_keys
          authorized_key: user=root
                          exclusive=no
                          key="{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

    # Apt, Yum, Dnf upgrade
        - name: upadate and upgrade
          raw: apt-get update && apt-get upgrade -y
          raw: yum update -y
          raw: dnf update -y


    - name: Install sudo
      apt:
        name: sudo
        state: present

    - name: Create "ansible" user
      user:
        name: ansible
        password: $6$LvTHj4o87bz50LRG$Vj.8B6.zUw.SM7DE7b9QjXAZtrUjaGXoCX1sYuBS8Zal67Gv7ibUW3TRcVXrXZSP5/LOJssFKcFI3FPEffp9g1
        shell: /bin/bash

    - name: Add user "ansible" to sudoers
      lineinfile:
        path: /etc/sudoers.d/ansible
        line: 'ansible ALL=(ALL) NOPASSWD: ALL'
        state: present
        mode: 0440
        create: yes
        validate: 'visudo -cf %s'


    # Install utility packages

        - name: install utils
          package:
            name: "{{ item }}"
          with_items:
            - sudo
            - tmux
            - man-db
            - curl
            - wget
            - tree
          ignore_errors: yes

        - name: install which
          package:
            name: which
          ignore_errors: yes


## Password Hash
Idempotent method to generate unique password hashes per system using a salt that is consistent between runs

    ansible localhost -m debug -a "msg={{ 'ansible' | password_hash('sha512', 65534 | random(seed=inventory_hostname) | string) }}"
