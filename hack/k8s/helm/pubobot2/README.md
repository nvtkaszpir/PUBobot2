# About

PUBobot2 helm chart

## Requirements

- kubernetes cluster with some mentally low resources like 1 CPU , 128MB ram, 10 Gb of disk for logs
- installed mysql or mariadb database somewhere
- helm3

## Preparing database

### Host db outside of k8s cluster

Set proper user/pass/fqdn/db_name in custom `values.yaml` file

## Host db in inside k8s cluster

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

create `db-private.yaml` with at least those values set:

```yaml
auth:
  database: pubobot
  username: pubobot
  password: pubobot-password
```

install it:

```bash
helm install --namespace bot bot1-sql bitnami/mariadb -f db-private.yaml --create-namespace
```

In about 1 minute database should be up and running.

## Configure bot

- Copy `values.yaml` as `values-private.yaml`.
- Edit the file
- alter section `config`
- set `DC_BOT_TOKEN`
- set `DC_OWNER_ID`
- set `DB_URI` to something like
  `DB_URI = "mysql://pubobot:pubobot-password@bot1-sql-mariadb:3306/pubobot"`
  if you used 'host db inside k8s cluster'
- tweak the rest of the conifg
- adjust container repo/image/tag especially if you use private networks

## Install

```bash
helm install bot . --namespace bot -f values-private.yaml
```

## Debug

See container logs.

## Advanced usage

You tell me.
