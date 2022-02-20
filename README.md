# Terraform Provider for Bitwarden

![Tests](https://github.com/maxlaverse/terraform-provider-bitwarden/actions/workflows/tests.yml/badge.svg?branch=main)
![Go Version](https://img.shields.io/github/go-mod/go-version/maxlaverse/terraform-provider-bitwarden)
![Releases](https://img.shields.io/github/v/release/maxlaverse/terraform-provider-bitwarden?include_prereleases)


The Terraform Bitwarden provider is a plugin for Terraform that allows to manage different kind of Bitwarden resources.
This project is not associated with the Bitwarden project nor 8bit Solutions LLC.

**[Explore the docs »][Terraform Registry docs]**

---

## Table of Contents
- [Supported Versions](#supported-versions)
- [Usage](#usage)
- [Developing the Provider](#developing-the-provider)
- [License](#license)

## Supported Versions
The plugin has been tested and built with the following components:
- [Terraform] >= 1.0.2
- [Bitwarden CLI] >= 1.19.1
- [Go] >= 1.17 (for development)
- [Docker] >= 4.2 (for development)

The provider might work with older versions but those haven't been tested.

## Usage

Detailed documentation for using this provider can be found on the [Terraform Registry docs].

```tf
# Bitwarden Credentials
variable "bw_password" {
  type        = string
  description = "Bitwarden Master Key"
  sensitive   = true
}

variable "bw_client_id" {
  type        = string
  description = "Bitwarden Client ID"
  sensitive   = true
}

variable "bw_client_secret" {
  type        = string
  description = "Bitwarden Client Secret"
  sensitive   = true
}

# Provider configuration
terraform {
  required_providers {
    bitwarden = {
      source  = "maxlaverse/bitwarden"
      version = "0.0.1"
    }
  }
  required_version = ">= 1.0.2"
}

provider "bitwarden" {
  master_password = var.bw_password
  client_id       = var.bw_client_id
  client_secret   = var.bw_client_secret
  email           = "test@laverse.net"
  server          = "https://vault.bitwarden.com"
}

# Save sensitive Terraform generated data to Bitwarden
resource "bitwarden_folder" "terraform-bw-folder" {
  name = "Terraform Generated"
}

resource "bitwarden_item_login" "vpn-read-only-userpwd" {
  name      = "VPN Read Only User/Password Access"
  username  = "vpn-read-only"
  password  = <some_other_plugin>.user-read-only.secret
  folder_id = bitwarden_folder.terraform-bw-folder.id
}

resource "bitwarden_item_secure_note" "vpn-read-only-certs" {
  name      = "VPN Read Only Certificate Access"
  notes     = <some_other_plugin>.user-read-only.private_key
  folder_id = bitwarden_folder.terraform-bw-folder.id
}

# Read sensitive information from Bitwarden
data "bitwarden_item_login" "mysql-root-credentials" {
  id = "ec4e447f-9aed-4203-b834-c8f3848828f7"
}

# Later to be accessed as
#   ... = data.bitwarden_item_login.mysql-root-credentials.username
#   ... = data.bitwarden_item_login.mysql-root-credentials.password

data "bitwarden_item_secure_note" "ssh-private-key" {
  id = "a9e19f26-1b8c-4568-bc09-191e2cf56ed6"
}

# ....

```

See the [examples](./examples/) directory for a full example.

## Developing the Provider

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (see [Requirements](#requirements) above).

To compile the provider, run `go install`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

To generate or update documentation, run `go generate`.

In order to run the full suite of Acceptance tests, run `make testacc`.

```sh
$ make testacc
```

## License

Distributed under the Mozilla License. See [LICENSE](./LICENSE) for more information.

[Terraform]: https://www.terraform.io/downloads.html
[Go]: https://golang.org/doc/install
[Bitwarden CLI]: https://bitwarden.com/help/article/cli/#download-and-install
[Docker]: https://www.docker.com/products/docker-desktop
[Terraform Registry docs]: https://registry.terraform.io/providers/maxlaverse/bitwarden/latest/docs