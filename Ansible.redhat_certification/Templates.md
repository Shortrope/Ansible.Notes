# Templates
- Can be dynamically completed using variables  
- Processed using Jinja2 template language - .j2 extension
- can change file properties - mode, owner, group

Example:

    ---
    - hosts: webservers
      tasks:
      - name: install apache
        yum: name=httpd state=latest
      - name: apache config file
        template: 
          src: /srv/httpd.j2
          dest: /etc/httpd.conf
          validate: ??


## File Format
- File extension: .j2
- Access to the same variables the play has access to

Example:

    ServerRoot {{ httpd_ServerRoot }}
    Listen {{ httpd_Listen }}

### Using Facts

    The IP address is: {{ ansible_default_ipv4.address }}
    OS Flavor:         {{ ansible_distribution }}