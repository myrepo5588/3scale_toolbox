# 3scale toolbox

3scale toolbox is a set of tools to help you manage your 3scale product. Using the [3scale API Ruby Client](https://github.com/3scale/3scale-api-ruby).

## Table of contents
* [Installation](#installation)
* [Usage](#usage)
   * [Copy a service](#copy-a-service)
   * [Update a service](#update-a-service)
   * [Import from CSV](#import-from-csv)
   * [Remotes](#remotes)
* [Development](#development)
   * [Testing](#testing)
* [Plugins](#plugins)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)

## Installation
Install the CLI:

    $ gem install 3scale_toolbox

## Usage

```shell
$ 3scale help
NAME
    3scale - 3scale CLI Toolbox

USAGE
    3scale <command> [options]

DESCRIPTION
    3scale CLI tools to manage your API from the terminal.

COMMANDS
    copy       3scale copy command
    help       show help
    import     3scale import command
    update     3scale update command
    remote     3scale CLI remote

OPTIONS
    -k --insecure      Proceed and operate even for server connections
                       otherwise considered insecure
    -v --version       Prints the version of this command
```

### Copy a service
Will create a new service, copy existing proxy settings, metrics, methods, application plans and mapping rules.

3scale instances can be either a [URL](docs/remotes.md#remote-urls) or the name of a [remote](docs/remotes.md).

Help message:

```shell
$ 3scale copy service --help
NAME
    service - Copy service

USAGE
    3scale copy service [opts] -s <src> -d <dst>
    <service_id>

DESCRIPTION
    Will create a new services, copy existing proxy settings, metrics,
    methods, application plans and mapping rules.

OPTIONS
    -d --destination=<value>             3scale target instance. Url or
                                         remote name
    -s --source=<value>                  3scale source instance. Url or
                                         remote name
    -t --target_system_name=<value>      Target system name

OPTIONS FOR COPY
    -h --help                            show help for this command
    -k --insecure                        Proceed and operate even for server
                                         connections otherwise considered
                                         insecure
    -v --version                         Prints the version of this command
```

```shell
3scale copy service NUMBER --source=foo --destination=https://access_token@foo2-admin.3scale.net
```

### Update a service

Will update existing service, update proxy settings, metrics, methods, application plans and mapping rules.

3scale instances can be either a [URL](docs/remotes.md#remote-urls) or the name of a [remote](docs/remotes.md).

Help message:

```shell
NAME
    service - Update service

USAGE
    3scale update service [opts] -s <src> -d <dst>
    <src_service_id> <dst_service_id>

DESCRIPTION
    Will update existing service, update proxy settings, metrics, methods,
    application plans and mapping rules.

OPTIONS
    -d --destination=<value>      3scale target instance. Url or
                                  remote name
    -f --force                    Overwrites the mapping rules by deleting
                                  all rules from target service first
    -r --rules-only               Updates only the mapping rules
    -s --source=<value>           3scale source instance. Url or
                                  remote name

OPTIONS FOR UPDATE
    -h --help                     show help for this command
    -k --insecure                 Proceed and operate even for server
                                  connections otherwise considered insecure
    -v --version                  Prints the version of this command
```

Example:

```shell
$ 3scale update service -s https://9874598743@source.example.com -d foo 3 2
```

### Import from CSV

Will create new services, metrics, methods, and mapping rules having as source comma separated values (CSV) formatted file.

3scale instances can be either a [URL](docs/remotes.md#remote-urls) or the name of a [remote](docs/remotes.md).

CSV header

```csv
service_name,endpoint_name,endpoint_http_method,endpoint_path,auth_mode,endpoint_system_name,type
```

File example

```csv
service_name,endpoint_name,endpoint_http_method,endpoint_path,auth_mode,endpoint_system_name,type
Movies ,Movies (Biography),GET,/movies/biography/,api_key,movies_biography,metric
Movies ,Movies (Drama),GET,/movies/drama/,api_key,movies_drama,method
```

Help message:

```shell
$ 3scale import csv -h
NAME
    csv - Import csv file

USAGE
    3scale import csv [opts] -d <dst> -f <file>

DESCRIPTION
    Create new services, metrics, methods and mapping rules from CSV
    formatted file

OPTIONS
    -d --destination=<value>      3scale target instance. Url or remote name
    -f --file=<value>             CSV formatted file

OPTIONS FOR IMPORT
    -h --help                     show help for this command
    -k --insecure                 Proceed and operate even for server
                                  connections otherwise considered insecure
    -v --version                  Prints the version of this command
```

Example:

```shell
3scale import csv --destination=https://provider_key@user-admin.3scale.net --file=examples/import_example.csv
```

### Remotes

Manage set of 3scale instances.

[Howto](docs/remotes.md)

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment. Run `bundle exec 3scale` to use the gem in this directory, ignoring other installed copies of this gem.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

### Testing

To run all tests run `rake`.

There are two kinds of tests:
* unit (see [spec/unit](spec/unit))
```bash
rake spec:unit
```

* integration (see [spec/integration](spec/integration)).
```bash
rake spec:integration
```

Integration tests can be run locally or against a real 3scale account.
When details of the account are set via environment variables,
integration tests are run agains given account.
Otherwise, tests are run locally with mocked 3scale clients.

The easiest way to set everything up is it to have a `.env` file in the root of the project with the following environment variables (set your own values):

```
ENDPOINT=https://your-domain-admin.3scaledomain
PROVIDER_KEY=abc123
VERIFY_SSL=true (by default true)
```

## Plugins

As of 3scale Toolbox 0.5.0, 3scale Toolbox will load plugins installed in gems or $LOAD_PATH. Plugins are discovered via Gem::find_files then loaded.
Install, uninstall and update plugins using tools like [RubyGems](https://guides.rubygems.org/rubygems-basics/) and/or [Bundler](https://bundler.io/).

[Make your own plugin](docs/plugins.md)

## Troubleshooting

* [SSL errors](docs/ssl_errors.md): If you run into SSL issues with the toolbox, you can take actions to resolve them.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/3scale/3scale_toolbox.

