# vault migrator

[release]: https://github.com/nebtex/vault-migrator/releases

[![GitHub release](http://img.shields.io/github/release/nebtex/vault-migrator.svg?style=flat-square)][release]
[![Go Report Card](https://goreportcard.com/badge/github.com/nebtex/vault-migrator)](https://goreportcard.com/report/github.com/nebtex/vault-migrator)

migrate or backup vault data between two physical backends. in one operation or in a cron job.

tested with: `vault v0.7`, `consul`, `dynamodb`

#### Links

* **⇩** [Download](#binaries)
* **⌧** [Docker](#docker) 

# Warnings

* Before you run this tool, make sure that you are not running vault in the destination backend

# Usage

create a `config.json` file with this structure

```json
{
  "to": {
    "name": "[[Backend Name]]",
    "config": "[[Backend Config]]"
  },
    "from": {
        "name": "[[Backend Name]]",
        "config": "{[[Backend Config]]"
    }
}
```

where `from`, is the source backend, and `to`  is the destination

## Examples:

> remember only use strings in the backend config values!!!

1. from dynamodb to consul

```json
{
  "to": {
    "name": "consul",
      "config": {
        "address": "127.0.0.7:8500",
        "path": "vault",
        "token": "xxxx-xxxx-xxxx-xxxx-xxxxxxxxx"
     }
  },
    "from": {
        "name": "dynamodb",
        "config": {
          "ha_enabled": "true",
          "table": "vault",
          "write_capacity": "1",
          "access_key": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
          "secret_key": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        }
    },
  "schedule": "@daily"
}
```

> this will backup each 24 hours your data in dynamodb to a consul instance. 

full list of storage backends and configuration options: [Vault Storage Backends](https://www.vaultproject.io/docs/configuration/storage/index.html)

`schedule` is optional if is not defined the command will run only once, for more documentation about is format please check [robfig/cron](https://godoc.org/github.com/robfig/cron)

## Binaries

[![Releases](https://img.shields.io/github/downloads/nebtex/vault-migrator/total.svg)][release]

#### OS X 

```shell
curl -LO https://github.com/nebtex/vault-migrator/releases/download/$(curl -s https://raw.githubusercontent.com/nebtex/vault-migrator/master/stable.txt)/vault-migrator_darwin_amd64.zip
```

#### Linux

```shell
curl -LO https://github.com/nebtex/vault-migrator/releases/download/$(curl -s https://raw.githubusercontent.com/nebtex/vault-migrator/master/stable.txt)/vault-migrator_linux_amd64.zip
```

#### Windows

```shell 
curl -LO https://github.com/nebtex/vault-migrator/releases/download/$(curl -s https://raw.githubusercontent.com/nebtex/vault-migrator/master/stable.txt)/vault-migrator_windows_amd64.zip
```

unzip and make the vault-migrator binary executable and move it to your PATH 

full list of downloads for other platforms [here][release]

#### Usage

```shell 
vault-migrator --config ${your_config_path}
```

## Docker

[![](https://images.microbadger.com/badges/image/nebtex/vault-migrator.svg)](https://microbadger.com/images/nebtex/vault-migrator "Get your own image badge on microbadger.com")
[![Docker Pulls](https://img.shields.io/docker/pulls/nebtex/vault-migrator.svg)](https://hub.docker.com/r/nebtex/vault-migrator/)

### linux amd64

```shell 
docker pull nebtex/vault-migrator:$(curl -s https://raw.githubusercontent.com/nebtex/vault-migrator/master/stable.txt)
``` 
#### Usage

```shell 
docker run -v ${your_config}:/etc/vault-migrator.json nebtex/vault-migrator
```
## Contribution

To contribute to this project, see [CONTRIBUTING](CONTRIBUTING).

## Licensing

*vault-migrator* is licensed under the APACHE License v2. See [LICENSE](LICENSE) for the full license text.

