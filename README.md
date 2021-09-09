# syslog

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/syslog/status.svg)](https://drone.osshelp.ru/ansible/syslog)

Installs selected syslog server (rsyslog/syslog-ng/etc) and creates system user if requested.

## Usage (example)

```yaml
  roles:
    - role: syslog
      create_user: true
      create_directory: true
      default_server: rsyslog
      extra_packages: [rsyslog-doc]
      syslog_accept_logs_from_containers: true
      syslog_containers_log_file: '/var/log/lxc-test.log'
      syslog_reception:
        enabled: true
        protocol: udp
        port: 514
      syslog_custom_params: [ 'if $programname startswith "test" then /var/log/test.log' ]
      syslog_generate_custom_cfg:
        enabled: true
        template_path: 'templates/osshelp.conf.j2'
        target_file_name: 'test-config.conf'
```

## Available parameters

### Main

| Param | Description |
| -------- | -------- |
| `syslog_setup` | Setup mode `full` (default) or `configure` |
| `create_user` | Boolean, whether to create system user. |
| `create_directory` | Boolean, whether to create /var/log (usually created by syslog server). |
| `install_server` | Whether to install selected syslog server. |
| `default_server` | Package name for selected syslog server. |
| `default_service` | Service name for selected syslog server. |
| `extra_packages` | List of additional packages with modules for selected syslog server. |
| `syslog_accept_logs_from_containers` | Boolean, when set to "true", the logs from containers will be accepted and saved to syslog_containers_log_file. Role will generate `/etc/rsyslog.d/osshelp.conf` with needed params. |
| `syslog_containers_log_file` | Absolute path to file where containers logs should be saved. |
| `syslog_custom_params` | Array with strings to add to `/etc/rsyslog.d/osshelp.conf`. Empty by default. |
| `syslog_install_logrotate` | Install logrotate. True by default |
| `syslog_reception` | Params to tell rsyslogd the port to listen on. |

### Misc

Generating configuration file from your own template.

| Param | Description |
| -------- | -------- |
| `syslog_generate_custom_cfg.enabled` | When set to "true", you can set your template and specify the target file name to create in `/etc/rsyslog.d`.
| `syslog_generate_custom_cfg.template_path` | Relative path to j2-template in your repository, that will be used for config generation. |
| `syslog_generate_custom_cfg.target_file_name` | Name to give to generated configuration file. |

## FAQ

None, so far.

## Useful links

- [Official site](https://www.rsyslog.com/)
- [Newbie guide](https://www.rsyslog.com/newbie-guide-to-rsyslog/)

## TODO

- detect existing syslog server (when packages_facts will be fixed in 2.8+?)
- re-create default subdirectories within /var/log (with correct permissions)
- we should somehow detect selected syslog server in tests 0_o
- remote logging setup support?

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
