# Ansible-Vault
Encrypt data or a file with a password  

    ansible-vault encrypt <file>
    ansible-vault rekey <file>          # change pw 
    ansible-vault edit <file>
    ansible-vault view <file>
    ansible-vault decrypt <file>

    ansible-vault encrypt_string 'The string'
    ansible-vault encrypt_string 'The string' -n meaning --vault-id dev@prompt
    ansible-vault encrypt_string 'The string' -n meaning --vault-id dev@filename

## Playbooks
Need a way to pass the password to a playbook  

    ansible-playbook --ask-vault-password   # depricated
    ansible-playbook --ask-vault-file       # depricated
    ansible-playbook --vault-id             # ver 2.4 allows multiple different pws
                                            # can use a label for a specific pw

## no_log
Use the `no_log` attrib in a playbook so the secrets are not displayed w the -v (verbose) option.

    ---
    hosts: localhost
    vars_files:
      - /home/user/vault/secrets
    tasks:
      - name: Output message
        shell: echo {{ message }} > /home/user/vault/deployed.txt
        no_log: true

## Example

    echo "The bird is the word" > secret.txt
    ansible-vault encrypt secret.txt

### --vault-id
File: secure.vars  
    message: "I am the walrus"  

File: vault.prod  
    hello1234  

Then encrypt the 'secrue.vars' file w the passwd in the vault.prod file

    ansible-vault encrypt --vault-id prod@vault.prod secure.vars

Call the playbook with the passwd

    ansible-playbook myencrypt.yml --vault-id prod@vault.prod




