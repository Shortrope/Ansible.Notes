# Ansible Facts

    ansible localhost -m setup -a "filter=*ipv4*"
    ansible localhost -m setup -a "*hostname*"

    ---
    - hosts: webservers
      vars:
        target_service: httpd
        target_state: started

      tasks:
      - name: Ensure target state
          service:
          name: "{{ target_service }}"
          state: "{{ target_state }}"
  
