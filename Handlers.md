# Handlers

Allows a action to be executed when a task performs a change.  
This can make a play more efficient.  
Will only be executed once for the play

    handlers:
    - name: attempt to restart httpd
      service:
        name: httpd
        state: restarted
      listen: "restart httpd"

The tasks can call the handler by its 'listen' value

    tasks:
    - name: add web content
      lineinfile:
        line: "<h1>Hey Buddy!</h1>"
        path: /var/www/html/index.html
      notify:
      - restart httpd