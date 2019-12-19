gudron.php_fpm_config
=====================

Role for create php-fpm config files

Role Variables
--------------

### General variables

  * `php_version: string`
    The php version for which you want to generate the configuration.

  * `conf_file_path: string`
    The path to the directory where you want to save the configuration files.

  * `pid: string`
    The path to file stored php-fpm process identificator.

  * `error_log: string`
    Path to error log file.

  * `log: dict`
    Dict with logs settings.

    * `level: string`
      Error log level. Possible values: alert, error, warning, notice, debug. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#log-level)

    * `limit: int`
      Log limit for the logged lines which allows to log messages longer than 1024 characters without wrapping. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#log-limit)

    * `buffering: boolean`
      Experimental logging without extra buffering. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#log-buffering)

  * `syslog: dict`
    Dict with syslog settings.

    * `facility: string`
      Used to specify what type of program is logging the message. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#syslog-ident)

    * `ident: string`
      Prepended to every message. If you have multiple FPM instances running on the same server, you can change the default value which must suit common needs. [Official php-fpm documentation](https://www.php.net/manual/en/install.fpm.configuration.php#syslog-facility)

  * `daemonize: boolean`
    Send FPM to background. Can contains only True or False boolean values.

  * `default_pools_parameters: dict`
    Default pool parameters. Like `user`, `group`, `request_terminate_timeout` and etc.

    This parameters will be passed to dependent [gudron.php_fpm_pool role](https://github.com/gudron/gudron.nginx_vhost).

    To find out all available variables check **gudron.php_fpm_pool** role [default variables](https://github.com/gudron/gudron.php_fpm_pool/blob/master/defaults/main/main.yml).

  * `pools: dict`
    Dictionary contains parameters of PHP-FPM pool. By default, parameters from `default_pools_parameters` are inherited. 

    This parameters will be passed to dependent [gudron.php_fpm_pool role](https://github.com/gudron/gudron.nginx_vhost).

    * `env_variables: list`
      List of environment variables which will be passed to pool process.

  * `disabled_functions_params: list`
    List of default disabled functions. [Official php-fpm documentation](https://www.php.net/manual/en/ini.core.php#ini.disable-functions).

    This parameters will be passed to dependent [gudron.php_fpm_pool role](https://github.com/gudron/gudron.nginx_vhost).

#### All supported variables: 

  * [defaults/main/main.yml](defaults/main/main.yml).
  * [defaults/main/php_fpm_params.yml](defaults/main/php_fpm_params.yml).
  * [defaults/main/pool_params.yml](defaults/main/pool_params.yml).

Dependencies
------------

  * gudron.php_fpm_pool - [Role for creating php-fpm pools config files](https://github.com/gudron/gudron.php_fpm_pool)

Instalation
-----------

Add **gudron.php_fpm_config** role to your *requirements* file.

```yaml
  - src: git@github.com:gudron/gudron.php_fpm_config.git
    scm: git
    version: master
```

Install roles via **ansible-galaxy** tool.

```bash
ansible-galaxy install -p roles -r requirements.yml
```

Example Playbook
----------------

    - hosts: example_project:&example_project_stage
      any_errors_fatal: "{{ any_errors_fatal | default(true) }}"
      gather_facts: false
      vars:
        ansible_connection: local
        ansible_become: no
        ansible_distribution: Debian
            
      roles:
        - name: gudron.php_fpm_config
          vars: 
            php_version: 7.3
            conf_file_path: /example/project/php-fpm/pool.d/sites-available/
            pid: /run/php-fpm.pid
            error_log: /var/log/php-fpm.log
            daemonize: "no"
            pools_params:
              example_pool_1:
                port: 9011
                user: example_user
                group: example_user
                slowlog: /path/to/slowlog/exmaple_pool_1.log
                env_variables:
                  - MYSQL_POOL1_WORKER_USER
                  - MYSQL_POOL1_WORKER_PASSWORD

              example_pool_2:
                port: 9012
                user: example_user_2
                group: example_user_2
                security:
                  limit_extensions:
                    - .php
                    - .php7
                env_variables:
                  - MYSQL_POOL2_WORKER_USER
                  - MYSQL_POOL2_WORKER_PASSWORD

License
-------

Apache 2.0
