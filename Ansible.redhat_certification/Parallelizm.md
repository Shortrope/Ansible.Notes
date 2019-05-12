# Parallelism
## Forks
default in ansible.cfg = 5

    forks = 5

Can set the number of forks with the `-f` option with cli command

## serial
The `serial` keyword can be used to ramp up or set number of forks

    ---
    - hosts: labservers
      serial: 10

Or Ramp up

    ---
    - hosts: labservers
      serial:
      - 1
      - 2
      - 50%
      max_fail_percentage: 30

