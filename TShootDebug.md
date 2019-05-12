# Trouble Shoot and Debug

- Debug module
- Register module

Print  information about in-progress plays.  
Like printing out variables in a script
## Debug
Takes ONE of two primary parameters
- msg: message printed to STDOUT
- var: print the content of a variable to STDOUT

Example:

    - debug
      var: inventory_hostname
    - debug
      msg: "System {{ inventory_hostname }} has uuid {{ ansible_product_uuid }}"
    - debug:
      var: ansible_processor

    # get all vars
    - debug:
      var: hostvars[inventory_hostname]

## Register
- Used to store task output  
- I also stores some attribs of the output  
- Can use the attribs to make decision on what to do next. 

Attributes:
- return code
- stdout
- stderr

Example:

    - hosts: all
      tasks:
      - shell: cat /etc/motd
        register: motd_contents
      - debug:
        var: motd_contents