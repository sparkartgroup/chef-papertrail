# Papertrail Cookbook

This cookbook configures the Papertrail [remote_syslog2](https://github.com/papertrail/remote_syslog2) agent. If you want to configure `rsyslog` for use with Papertrail, check out the [documentation on the Papertrail website](http://help.papertrailapp.com/kb/configuration/configuring-remote-syslog-from-unixlinux-and-bsdos-x/).

## Supported Platforms

* RHEL 6 / CentOS 6
* RHEL 7 / CentOS 7
* Amazon Linux 2017.03
* Ubuntu 14.04
* Ubuntu 16.04
* Debian 9

## Usage

### Quickstart

1. Set the attributes `node['papertrail']['destination_host']`, `node['papertrail']['destination_port']`, and at least one file/directory in `node['papertrail']['files']`.
2. Include `papertrail` in your node's `run_list`:

```json
{
  "run_list": [
    "recipe[papertrail]"
  ]
}
```

This will install `remote_syslog2` with the configured settings you set in the Chef node attributes.


## Recipes & their attributes

This cookbook only has one recipe, which does all setup and configuration. There are a number of attributes you can configure, all of which mirror the configuration items found in the `remote_syslog2` [README](https://github.com/papertrail/remote_syslog2#configuration).

- `node['papertrail']['files']`

  **Type:** Array

  A list of files or patterns to send to Papertrail. At least one entry is required.

  Example:
  ```ruby
    node['papertrail']['files'] = [
      '/tmp/test.log',
      '/srv/foo.txt',
      '/var/log/*.bar'
    ]
  ```

  If you want to tag a file/path, the structure is slightly different:
  ```ruby
    node['papertrail']['files'] = [
      '/tmp/test.log',
      '/srv/foo.txt',
      {'path' => '/srv/foo.txt', 'tag' => 'my_tag'}
    ]
   ```

- `node['papertrail']['exclude_files']`

  **Type:** Array

  A list of files or patterns to exclude.

  Example:
  ```ruby
    node['papertrail']['exclude_files'] = [
      '/tmp/exlude.log',
      '/srv/dont-include.log',
      '/var/log/skip-me.log'
    ]
  ```

- `node['papertrail']['exclude_patterns']`

  **Type:** Array

  A regex of log message patterns to exclude.

  Example:
  ```ruby
    node['papertrail']['exclude_patterns'] = ['\d+ things']
  ```
- `node['papertrail']['hostname']`

  **Type:** String

  Override the default hostname.

  Example:
  ```ruby
    node['papertrail']['hostname'] = 'my-super-awesome-hostname'
  ```

- `node['papertrail']['destination_host']`, `node['papertrail']['destination_port']`, & `node['papertrail']['destination_protocol']`

  **Type:** String

  The Papertrail host, port, and protocol for sending your logs to. These are required. Destination and port default to empty, while Protocol defaults to `tls`.

  Example:
  ```ruby
    node['papertrail']['destination_host'] = 'YOUR-HOST-HERE'
    node['papertrail']['destination_port'] = 'YOUR-PORT-HERE'
    node['papertrail']['destination_protocol'] = 'tls'
  ```

- `node['papertrail']['new_file_check_interval']`

  **Type:** Integer

  Overrides the default file check interval.

  Example:
  ```ruby
    node['papertrail']['new_file_check_interval'] = 30
  ```

- `node['papertrail']['version']`

  **Type:** String

  Use a different version of `remote_syslog` than the default. The default is specified in `map.jinja`.

  Example:
  ```ruby
    node['papertrail']['version'] = '0.18'
  ```

## Testing

See [TESTING.md](TESTING.md)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

## License and Authors

**License:** See [LICENSE](LICENSE.md)

**Author:** Mike Julian (@mjulian)
