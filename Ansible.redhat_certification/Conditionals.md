# Conditionals

## `handlers`
Take action when called  
Called with `notify`

    - hosts: webservers
      become: yes

      handlers:
      - name: restart apache
        service:
          name: apache2
          state: restarted
        listen: "bounce web"

      tasks:
        -name: template config file
         template:
           src: template.js
           dest: /etc/foo.comf
         notify: "bounce web"


The task can call the handler(s) with `notify`   
The `notify` only happens if the template is changed   
Can call multiple handlers

      notify:
      - bounce web
      - other action

## When
Check if a condition is true

    - hosts: labservers
      become: yes
      tasks:
        - name: edit index
          lineinfile:
            path: /var/www/html/index.html
            line: "I'm back!!"
          when:
            - ansible_hostname == "adeb"

## With_items
Loop thru items

    - hosts: local
      become: yes
      tasks:
        - name: create users
          user:
            name: "{{item}}"
          with_items:
            - sam
            - josie
            - bob

## With_files