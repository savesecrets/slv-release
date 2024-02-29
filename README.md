# SLV - Secure Local Vault
Secure Local Vault - SLV (a.k.a Secrets Launch Vehicle 🔐🚀) is a tool to manage secrets locally in a secure manner. It is designed to be used by developers to manage secrets along with their code so that they can be shared with other developers and services in a secure manner.

SLV is designed based on the following **key principles**
 - Anyone can add or update secrets, however will not be able to read them unless they have access to the vault
 - An environment should have a single identity that will give access to all necessary secrets from any vault shared with it

## Installation
Download the latest SLV binary from the [releases](https://github.com/savesecrets/slv/releases/latest) page and add it to your path.

SLV can also be installed with brew using the following command
```zsh
brew install savesecrets/tap/slv
```

## Usage

#### Create a new profile
```sh
$ slv profile new -n amagi

Created profile:  amagi
```

#### Create a new environment
```sh
$ slv env new service -n alice -e alice@example.com --add

ID (Public Key):  SLV_EPK_AEAUKAELRTIL2YIXNP7NYTYQMHUX77BWK2LXSKXN4GHSUECDNEJ7XFECLE
Name:             alice
Email:            alice@example.com
Tags:             []

Env Definition:  SLV_EDS_AF4JYNGKIFVYGMAYQDQ774U5MUDRSBDSTK5G7UZFBPMYYW5ECRETKSBAKFISVFOS75PKJ5HY6I7FPEHHSN3S3MY3KAUPSX4DSI2QSJQVJOIP7KUCY522DBJEUJLPLT3XLZUUFUT7CZZV2MRNLY77HMWC5RO6AF6RD6MHDBAIQQERMKAY55NAWELAGDHD766NLZGJRPD5NHD3BP3BKXN3J26FZ3V4GK6TF5AA7RYI4Q6K5LVTOPVINTQNHVIIBWZ5AAAP775I7Q3QS

Secret Key: SLV_ESK_AEAEKAHBIONE3QIIWFXFRNJPE6A6AYL527QW4OF4HWWFDOE5E4XR5LO2WI
```

#### Create a vault
- To create a vault and share it with the environment `alice`, use the following command
```sh
$ slv vault new -v test.slv.yaml -s alice

Created vault: test.slv.yaml
```
- To create a K8s compatible vault, use the following command
```sh
$ slv vault new -v test.slv.yaml -s alice --k8s production

Created vault: test.slv.yaml
```

#### Add secrets to the vault
```sh
$ slv secret put -v test.slv.yaml -n db_password -s "super_secret_pwd"

Added secret: db_password to vault: test.slv.yaml
```

#### Get secrets from the vault
Set the environment variable `SLV_ENV_SECRET_KEY` to the secret key generated in the previous step
```sh
$ export SLV_ENV_SECRET_KEY=SLV_ESK_AEAEKAHBIONE3QIIWFXFRNJPE6A6AYL527QW4OF4HWWFDOE5E4XR5LO2WI
$ slv secret get -v test.slv.yaml -n db_password

super_secret_pwd
```

#### Share the vault with other environments
Ensure that the current environment has access to the vault in order to share it with other environments
```sh
$ slv vault share -v test.slv.yaml -s bob

Shared vault: test.slv.yaml
```
Once shared, the other environments can access the vault using their respective secret keys

## Integrations
Some of the integrations that SLV supports currently are:
- GitHub Actions
    - [`slv-setup-action`](https://github.com/savesecrets/slv-setup-action)
    - [`slv-secrets-action`](https://github.com/savesecrets/slv-secrets-action)
- [Kubernetes Operator](/operator/README.md)