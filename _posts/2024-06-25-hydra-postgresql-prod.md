---
layout: single
title: Install Hydra with PostgreSQL
tags:
  - fedora
  - hydra
  - postgresql
  - docker
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to create Minikube with KVM  
---
# Preparation

```bash
export SECRETS_SYSTEM=`export LC_CTYPE=C; cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1`
export HYDRA_VERSION=v2.2.0
export HYDRA_IMAGE=oryd/hydra:v2.2.0-distroless
export CONSENT_IMAGE=oryd/hydra-login-consent-node:$HYDRA_VERSION

export NETWORK_NAME=hydranet
export POSTGRESQL_NAME=ory-hydra-postgres
export HYDRA_ADMIN=hydra-admin
export HYDRA_PUBLIC=hydra-public
export PUBLIC_PORT=4444
export ADMIN_PORT=4445

export DSN=postgres://hydra:secret@$POSTGRESQL_NAME:5432/hydra?sslmode=disable
```

# Create network

```bash
docker network create hydranet
```

# PostgreSQL

```bash
docker run \
  --network hydranet \
  --name $POSTGRESQL_NAME \
  -e POSTGRES_USER=hydra \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_DB=hydra \
  -d postgres:16.3-alpine
```

# Hydra

## Prepare Data in DB 

```bash
docker run -it --rm \
  --network $NETWORK_NAME \
  $HYDRA_IMAGE \
  migrate sql --yes $DSN
```

## Run Hydra

### Consensus
Let's pull:

```bash
docker pull $HYDRA_IMAGE
```

### ALL IN ONE RUN

Run all in one:

```bash
docker run -d \
  --name hydra-all \
  --network $NETWORK_NAME \
  -p $PUBLIC_PORT:4444 \
  -p $ADMIN_PORT:4445 \
  -e SECRETS_SYSTEM=$SECRETS_SYSTEM \
  -e DSN=$DSN \
  -e URLS_SELF_ISSUER=https://localhost:4444/ \
  -e URLS_CONSENT=http://localhost:9020/consent \
  -e URLS_LOGIN=http://localhost:9020/login \
  $HYDRA_IMAGE serve all
```

### Run Admin and Public separately

#### Admin

```bash
docker run -d \
  --name $HYDRA_ADMIN \
  --network $NETWORK_NAME \
  -p $ADMIN_PORT:4445 \
  -e SECRETS_SYSTEM=$SECRETS_SYSTEM \
  -e DSN=$DSN \
  -e URLS_SELF_ISSUER=https://localhost:4444/ \
  -e URLS_CONSENT=http://localhost:9020/consent \
  -e URLS_LOGIN=http://localhost:9020/login \
  $HYDRA_IMAGE serve admin
```

#### Public

```bash
docker run -d \
  --name $HYDRA_PUBLIC \
  --network $NETWORK_NAME \
  -p $PUBLIC_PORT:4444 \
  -e SECRETS_SYSTEM=$SECRETS_SYSTEM \
  -e DSN=$DSN \
  -e URLS_SELF_ISSUER=https://localhost:4444/ \
  -e URLS_CONSENT=http://localhost:9020/consent \
  -e URLS_LOGIN=http://localhost:9020/login \
  $HYDRA_IMAGE serve public
```

# Hydra Consent UI

Let's pull

```bash
docker pull $CONSENT_IMAGE
```

Run it:

```bash
docker run -d \
  --name hydra-consent \
  -p 9020:3000 \
  --network $NETWORK_NAME \
  -e HYDRA_ADMIN_URL=https://$HYDRA_ADMIN:$ADMIN_PORT \
  -e NODE_TLS_REJECT_UNAUTHORIZED=0 \
  $CONSENT_IMAGE
```

# Example

## Let's make this simple

```bash
alias hydra="docker run --rm -it   -e HYDRA_ADMIN_URL=https://${HYDRA_ADMIN}:${ADMIN_PORT}   --network $NETWORK_NAME   ${HYDRA_IMAGE}"
```

## Create A Client

```bash
client=$(hydra create client \
    --endpoint http://127.0.0.1:4445/ \
    --format json \
    --grant-type client_credentials)
```

## Create An OAuth Client

```bash
hydra create oauth2-client \
  -e http://${HYDRA_ADMIN}:${ADMIN_PORT} \
  --name "hydra-client-eg" \
  --redirect-uri http://127.0.0.1:9010/callback \
  --grant-type authorization_code,refresh_token,client_credentials,implicit \
  --response-type token,code,id_token \
  --scope openid,offline,photos.read \
  --skip-tls-verify
```

Get Client secret and ID. Put them in CLIENT_SECRET and CLIENT_ID accordingly.

## Perform authentication

```bash
hydra perform client-credentials \
  --endpoint http://hydra-public:4444/ \
  --client-id "${CLIENT_ID}" \
  --client-secret "${CLIENT_SECRET}"

```

## Delete All

```bash
for i in $(hydra ls oauth2-clients   -e http://${HYDRA_ADMIN}:${ADMIN_PORT} --format json | jq -r ".items[].client_id" ) ; do 
  hydra -e http://$HYDRA_ADMIN:$HYDRA_PORT delete oauth2-client $i; 
done

```

# Troubleshooting

## Hydra Help

```bash
docker run -it --rm $HYDRA_IMAGE help serve
```

## Check ports

```bash
docker container ls --format "table {{.ID}}\t{{.Names}}\t{{.Ports}}" -a
```
