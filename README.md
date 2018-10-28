pgpool2 ansible role
====================

sets up pgpool2 for Postgres connection pooling, load balancing etc


Role Variables
--------------

- `pgpool_install_os` - install pgpool2 from default distribution

- `pgpool_listen_address` (default: 127.0.0.1)
- `pgpool_port` (default: 5432)
- `pgpool_backends_version` - needed for failover script & auto-determining
  backends' PGDATA; defaults to 9.6
- `pgpool_backends` - either list of hosts/ips or dictionary:
    - `host`
    - `port` (default: 5432)
    - `weight` (default: 1)
    - `directory` - clusters' data directory (defaults to default main cluster of given version directory)
- `pgpool_num_init_children` (default: 32)
- `pgpool_max_pool` (default: 10)
- `pgpool_load_balance_mode` - whether to delegate some SELECT queries to replicas
- `pgpool_master_slave_enabled` - let pgpool figure out which backend is master or slave
- `pgpool_master_slave`
    - `database` (default: `pgpool`) note: you need to create it prior
    - `user` (default: `pgpool`) note: as above
    - `password` (omitted by default)
    - `delay_threshold` - threshold before not dispatching query to standby node in bytes
- `pgpool_healthcheck_enabled` - check backends periodically if they're still alive
- `pgpool_healthcheck`
    - `period` - in seconds (default: 5)
    - `timeout` (default: 1) in seconds
    - `max_retries` - number of tries failed healthcheck before giving up (default 0 which means infinite)
    - `database` - database for healthcheck (default: pgpool) note: you need to create it prior
    - `user` - user to perform healthcheck (default: pgpool) note: as above 
- `pgpool_failover_enabled` - install failover script and make pgpool use it in case of backend failure   
- `pgpool_failover`
    - `on_backend_error` - fail over on backend error (apart from possible health check) (default: yes)
    - `detach_false_primary` (default: no)
    - `search_primary_node_timeout` - timeout in seconds for searching primary node (default: 0 which means infinite) 
check `defaults/main.yml` for more

Example Playbook
----------------

    - hosts: web
      roles:
         - role: pgpool
           pgpool_load_balance_mode_enabled: yes
           pgpool_master_slave_mode: no
           pgpool_backends:
              - host: postgres0.localnet
              - host: postgres1.localnet


License
-------

BSD
