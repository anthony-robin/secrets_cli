[![Gem Version](https://badge.fury.io/rb/secrets_cli.svg)](https://badge.fury.io/rb/secrets_cli)

# SecretsCli

This is a CLI for easier use of [vault](https://www.vaultproject.io/)

There is also a mina plugin [mina-secrets](https://github.com/infinum/mina-secrets)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'secrets_cli'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install secrets_cli

## Prerequisites

`vault` must be installed on system. This gem adds a dependency to `vault-binaries` which will install `vault` for you.

The following environment variables need to be set:

For `vault` itself:

    VAULT_ADDR - this is an address to your vault server

For `secrets_cli`:

    SECRETS_VAULT_AUTH_METHOD - this is auth method ('github' or 'token' supported for now)
    SECRETS_VAULT_AUTH_TOKEN - this is vault auth token

For github token you only need `read:org` permissions.

## Usage

All commands have `--help` with detailed descriptions of options.
Some of the commands have `--verbose` switch which will print out the commands it run.

### Init

    $ secrets init

This will create `.secrets` file with project configuration. The command will ask you all it needs to know if you do not
supply the config through options.

Example of the `.secrets`:

    ---
    :secrets_file: config/application.yml # file where your secrets are kept, depending on your environment gem (figaro, dotenv, etc)
    :secrets_repo: rails/my_project/      # vault 'repo' where your secrets will be kept.
    :secrets_field: secrets               # a field in vault repo where the contents of secrets_file will be written

### Auth

    $ secrets auth

You need to first authenticate yourself on vault server to be able to read and write.
Needs to be done only _once_ for specific token.

### Repos and environments

Next 3 commands read and write to your project repo in vault. The value of the repo is generated by
secrets_repo + environment. Example:

      `rails/my_project/development`

Environment is `development` by default, but it can be overwriten by passing `--environment` option, or setting `RAILS_ENV` environment variable.

### Read

    $ secrets read

This will only read from vault.

Example of executed command:

    vault read rails/my_project/development

### Pull

    $ secrets pull

This will pull from vault and write to your secrets file.

### Push

    $ secrets push

This will push from your secrets file to vault.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/infinum/secrets_cli. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

