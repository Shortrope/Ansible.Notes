# Error Handling
- Ignoring acceptable errors
- Defining failure conditions
- Defining what 'Changed' means
- Block/Rescue/Always - like a try/catch


## Ignoring acceptable errors

    ---
    - hosts: local
      tasks:
      - name: get files
        get_url:
          url: "http://{{item}}/index.html"
          dest: "/tmp/{{item}}"
        ignore_errors: yes
        with_items:
        - 10.1.1.12
        - 10.1.1.13
        - 10.1.1.14


## Defining failure conditions
## Defining what 'Changed' means
## Block/Rescue/Always
Like try/catch/finally

    ---
    - hosts: local
      tasks:
      - name: get file
        block:
          - get_url:
              url: "http://10.1.1.13/index.html"
              dest: "/tmp/index.html"
        rescue:
          - debug: msg="The file does not exist!"
        always:
          - debug: msg="Play done."


Lab:

    ---
    - hosts: localhost
      tasks:
      - name: Get transaction list
        block:
        - get_url:
            url: "http://myapps.com/transaction_list
            dest: /home/ansible/transaction_list
        - debug:
            msg: "File downloaded"
        - replace:
            path: /home/ansible/transaction_list
            regexp: '#BLANKLINE'
            replace: "\n"
            backup: yes
        rescue:
        - debug:
            msg: "myapps.com appears to be down. Try again later."
        always:
        - debug:
            msg: "Attempt Completed"



