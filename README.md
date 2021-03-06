# ReBirth

[![Build Status](https://travis-ci.org/cryptape/re-birth.svg?branch=master)](https://travis-ci.org/cryptape/re-birth)
[![codecov](https://codecov.io/gh/cryptape/re-birth/branch/master/graph/badge.svg)](https://codecov.io/gh/cryptape/re-birth)
[![Documentation](http://img.shields.io/badge/docs-rdoc.info-blue.svg)](https://www.rubydoc.info/github/cryptape/re-birth/master)

English | [简体中文](./README-CN.md)

A blockchain explorer cache server for [CITA](http://docs.citahub.com).

Now only support CITA v0.22 and above.

⚠️ NOTE: If you upgrade your chain to 0.18 from 0.17, after upgrade, you should stop sync task(`rake daemons:sync:stop`) and run `rake zero18:update` to update your old data, and start your sync task (`rake daemons:sync:start`), then restart your server 😄

⚠️ NOTE: when update this version, you should run `bundle exec rake event_logs:fix_old` to sync event logs that it's transaction already saved.

⚠️ NOTE: add `errorMessage` to `transactions`, run `bundle exec rake transactions:add_error_message` to add.

⚠️ NOTE: If you upgrade your chain to 0.20, after upgrade, you should stop sync task(`rake daemons:sync:stop`) and run `rake zero20:update` to update your old data, and start your sync task (`rake daemons:sync:start`), then restart your server

## Docker

If you just want to run this, just use [docker 🐳](https://docs.docker.com/install)

Remember to run `rails secret` to generate secret key and write in `.env.local` (read `.env` for more info)
Remember to change your sidekiq username and password in `.env.local`

⚠️ IMPORTANT: your database data will save at docker/data

⚠️ IMPORTANT: your redis data will save at docker/redis

App will run at http://localhost:8888

```shell
$ make setup # run this when you first run this app
$ make up # this command will run docker daemon.
$ make update # when you update the app.
```

you can get more info from Makefile

## Packages

- [postgresql](https://www.postgresql.org/) 9.4 and above
- install [secp256k1](https://github.com/bitcoin-core/secp256k1.git)

  ```shell
  $ cd re-birth/tmp && git clone https://github.com/bitcoin-core/secp256k1.git && cd secp256k1 && ./autogen.sh && ./configure --enable-module-recovery && make && sudo make install && cd ../..
  ```

## Initial Project

```shell
$ bundle
$ touch .env.local (overwrite `.env` config if you need in `.env.local`, such as DB_USERNAME, DB_PASSWORD and CITA_URL...)
$ rails db:setup (or rails db:create db:migrate db:seed)
```
The default value of `CITA_URL` is: `localhost:1337`

Please make sure that the cita blockchain is RUNNING


## Running Test

```shell
$ touch .env.test.local (test env will not read `.env.local` file, overwrite in `.env.test.local`)
$ rails spec
```

## Run Project

```shell
$ rails s

# start sync process
$ rails daemons:sync:start
# run `rails daemons:sync:stop` to stop it
# run `rails daemons:sync:restart` to restart it
# run `rails daemons:sync:status` to see status
```

## Event Log Processor

Create a yaml file with `.yml` suffix in `config/customs`, see `config/customs/event_log.yml.sample` for more detail. For example, your file name is `contracts.yml`, you can run `bundle exec rake event_log:create[contracts]` to create your table and now restart sync task `bundle exec rake daemons:sync:restart` to listen your contact 😝

We'll both use `address` and `topics` to select the logs by jsonrpc interface [`getLogs`](http://docs.citahub.com/en-US/cita/rpc-guide/rpc#getlogs)

## Deploy

You can deploy this via [mina](https://github.com/mina-deploy/mina)

```shell
# replace `dev` with you env
$ mina dev deploy
$ mina dev 'rake[daemons:sync:start]'
```

## Build Doc

```shell
$ bundle exec yard doc
$ bundle exec yard server
```

##How to view log

Under `log` folder you can the `production.log` and `sidekiq.log` file
```
log
├── production.log
└── sidekiq.log
```

## API Doc
See [API Doc](./API_DOC.md)


## Contributing

### Creating a Bug Report

open a new issue: [https://github.com/citahub/re-birth/issues/new](https://github.com/citahub/re-birth/issues/new)

with your version info

### Tech Stack

Read [docs/tech_stack.md](docs/tech_stack.md) to know the programming languages, frameworks, and tools that developers use to build this software.

### Get source

```
git clone git@github.com:citahub/re-birth.git
```

### Coding style

#### Coding style for Ruby

* coding style guide: [https://rubystyle.guide](https://rubystyle.guide)
* linter: [RuboCop](https://github.com/bbatsov/rubocop)
* code formatter: $ rubocop -x


#### Coding style for Docker

* coding style guide: https://github.com/Haufe-Lexware/docker-style-guide
* formatter: https://www.fromlatest.io/
* best-practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

#### Coding style for Makefile

* coding style guide: https://style-guides.readthedocs.io/en/latest/makefile.html
* tutorial: https://makefiletutorial.com/
* conventions: https://www.gnu.org/prep/standards/html_node/Makefile-Conventions.html
* best-practices: https://suva.sh/posts/well-documented-makefiles/


### Running the tests

```
$ rails spec
```

### Commit your changes

#### Workflow

[GitHub Flow](https://help.github.com/en/articles/github-flow), [Understanding the GitHub flow](https://guides.github.com/introduction/flow/)

#### git style guide

use [git-style-guide](https://github.com/agis/git-style-guide) for Branches, Commits,Messages, Merging
