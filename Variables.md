# Variables
- Letters, Numbers, Underscores

## Command line
    --extra-vars or -e
    ansible-playbook web.yml -e "target_service=httpd"

## Playbook
### vars section
    vars:
      target_service: httpd
      target_state: started

    tasks:
    - name: Ensure target state
        service:
        name: "{{ target_service }}"
        state: "{{ target_state }}"
  
### var_files
    var_files:
        myvars.yml

_myvars.yml_

    target_service: httpd
    target_state: started

## Variable scope