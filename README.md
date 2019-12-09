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

  * `daemonize: string`
    Send FPM to background. Can contains only "on" or "off" string values.

  * `default_pools_params: dict`
    Default pool parameters. Like `user`, `group`, `request_terminate_timeout` and etc.

    This parameters will be passed to dependent [gudron.php_fpm_pool role](https://github.com/gudron/gudron.nginx_vhost).

    Еo find out all available variables check **gudron.php_fpm_pool** role [default variables](https://github.com/gudron/gudron.php_fpm_pool/blob/master/defaults/main/main.yml).

  * `pools_params: dict`
    Dictionary contains parameters of PHP-FPM pool. By default, parameters from `default_pools_params` are inherited. 

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
        - name: gudron.nginx_config
          vars: 
            conf_file_path: /any/path/to/nginx/config/files/directory/
            virtual_hosts_destintion_path: /example/project/nginx/conf.d/sites-available/
            proxy_params:
              http_version: "1.0"
              buffering: "on"
              buffers_count: 4
              buffer_size: 256k
              buffer_size_first_response: 512k
              busy_buffers_size: 512k
              connect_timeout: 30s
              read_timeout: 31s
            virlual_hosts_params:
              api:
                port: 80
                domain: api.example.com
                type: proxy
                root: /example/project/app/static/dir/
                status_path: ngxstatus
                headers:
                  X-Content-Type-Options: nosniff
                  Strict-Transport-Security: max-age=31536000; includeSubDomains
                  X-Frame-Options: SAMEORIGIN
                proxy_params:
                  X-Real-IP: $remote_addr
                include_params:
                  - /etc/nginx/conf.d/include_file.conf
                backends: 
                  - address: api.example.internal
                    port: 8080

License
-------

Apache 2.0
