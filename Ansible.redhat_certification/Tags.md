# Tags: Running selective tasks

    ---
    - hosts: web
      become: yes
      tasks:
      - name: deploy app binary
        copy: 
          src: /home/usr/apps/hello
          dest: /var/www/html/hello
        tags: 
        - webdeploy

    - hosts: db
      become: yes
      tasks:
      - name: deploy db script
        copy: 
          src: /home/usr/apps/scripts.sql
          dest: /opt/db/scripts/script.sql
        tags:
        - dbdeploy

Run with

    ansible-playbook deploy.yml --tags dbdeploy
    ansible-playbook deploy.yml --skip-tags dbdeploy