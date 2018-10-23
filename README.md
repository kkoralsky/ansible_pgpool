pgpool2 ansible role
====================

sets up pgpool2 for Postgres connection pooling, load balancing etc


Role Variables
--------------

check `defaults/main.yml`

Example Playbook
----------------

    - hosts: web
      roles:
         - role: pgpool
           pgpool_load_balance_mode: yes
           pgpool_master_slave_mode: no
           pgpool_backends:
              - host: postgres0.localnet
              - host: postgres1.localnet


License
-------

BSD
