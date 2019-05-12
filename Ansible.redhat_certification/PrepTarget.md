# Prep Target Machine

- SSH and accept host key --accept-hostkey
- Python2

Python Minimal install

    apt install python-minimal -no-install-recommends

- ansible.cfg: host_key_checking = False

## Prep playbook

    ansible-playbook prep.yml -u ubuntu -k --ask-sudo-pass

_prep.yml_

    ---
    - hosts: newbies
      gather_facts: False
      remote_user: ubuntu
      become: yes
      become_user: root
      become_method: sudo

      tasks:
        - name: Install Python
          raw: test -e /usr/bin/python || (apt update && apt install -y python)

        - name: Fancy way of doing authorized_keys
          authorized_key: user=root
                          exclusive=no
                          key="{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

        #- name: Create /root/.ssh
        #  file: path=/root/.ssh state=directory mode=0700

        #- name: Create authorized_keys
        #  lineinfile: dest=/root/.ssh/authorized_keys line="{{ lookup('file', '~/.ssh/id_rsa.pub') }}"