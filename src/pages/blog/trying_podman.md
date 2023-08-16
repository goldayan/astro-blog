---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Learning postgres using podman
description: Spinning postgres container in podman.
date: 2023-08-16
author: "Gold Ayan"
category: "Container"
tags:
  - Podman
  - Postgres
---

I thought of learning postgres. Instead of installing in the host machine how difficult it would be to spin up the container. Let's find out.

## Installing podman
- I installed the podman that is available in ubuntu repo
```
sudo apt install podman
```
- Note: you won't get the latest podman

## Let's check podman
- Let's pull the postgres container
```
podman pull postgres
```
- Nothing happened, so i tried `podman search postgres` which gave me the following error
```
ERRO[0000] User-selected graph driver "vfs" overwritten by graph driver "overlay" from database - delete libpod local files to resolve 
```
- After some googling and github issue surfing i found `fuse-overlayfs` is missing in my system, which caused the above error
```
sudo apt install fuse-overlayfs
```
- Only install this if you face error, by default it should be installed along with podman.
- source: https://unix.stackexchange.com/questions/701784/podman-no-longer-searches-dockerhub-error-short-name-did-not-resolve-to-an

## Pod initialize
- we will spin the container inside the pod, later if we want we can migrate it to kubernetes :p.
- Creating the pod
```
podman pod create --name postgres-pod -p 9876:80
```
- Install postgres in the pod
```
podman run --pod=postgres-pod \
-v ~/Documents/Postgres:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=goldayan \
-e POSTGRES_USER=goldayan \
--name db \
-d postgres
```
- I got the following error `short-name "postgres" did not resolve to an alias and no unqualified-search registries are defined in "/etc/containers/registries.conf"`
- I found the answer for this issue in the following stackoverflow answer, https://unix.stackexchange.com/questions/701784/podman-no-longer-searches-dockerhub-error-short-name-did-not-resolve-to-an
- `NOTE: RISK OF USING UNQUALIFIED IMAGE NAMES` We recommend always using
fully qualified image names including the registry server (full dns
name), namespace, image name, and tag (e.g.,
registry.redhat.io/ubi8/ubi:latest). Pulling by digest (i.e.,
quay.io/repository/name@digest) further eliminates the ambiguity of
tags
- Run the following command, let's replace postgres with docker.io/postgres.
- Telling it to fetch the image from docker.io registery.
```
podman run --pod=postgres-pod \
-v ~/Documents/Postgres:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=goldayan \
-e POSTGRES_USER=goldayan \
--name db \
-d docker.io/postgres
```

- pgadmin web version
```
podman run --pod=postgres-pod \
-e 'PGADMIN_DEFAULT_EMAIL=me@goldayan.in' \
-e 'PGADMIN_DEFAULT_PASSWORD=goldayan' \
--name pgadmin \  # name the container
-d docker.io/dpage/pgadmin4   # the image we are pulling
```
- Login to pgadmin using the following url `localhost:9876`
- Enter the creds, same as what you given for PGADMIN_DEFAULT_EMAIL and PGADMIN_DEFAULT_PASSWORD when running pgadmin container
- After SignIn, Press add new server
- Give any name for the server and for the connection type `localhost` for server and `5432` as port number.
- Maintenance database will be postgres by default.
- Username and password will be what you specified in the POSTGRES_USER and POSTGRES_PASSWORD when running postgres container.
- Press save.

## Pod commands
- Status of the pod
```
podman pod stats postgres-pod
```
- Pause and unpause the pod
```
podman pod pause postgres-pod
podman pod unpause postgres-pod
```
- Start and stop the pod
```
podman pod start postgres-pod
podman pod stop postgres-pod
```
- Last but not least, generate kubeconfig from the pod
```
podman generate kube postgres-pod >> postgres-pod-conf.yml
```

## Log into postgres container
- we will log into the postgres container and use the psql command
```
podman exec -it db bash
```
- psql command to connect to the db
```
psql -U goldayan
```
- we use goldayan because that is the username we used previously during creating postgres container.

## When importing csv file in pgadmin
- Before I upload the csv file i removed the header
- Select the upload csv file then press import, voila... imported.

## Reset the podman [WARN]
- To delete all the images, containers, pods and cache
```
podman system reset
```
- Just delete containers
```
sudo rm -rf ~/.local/share/containers/
```

## Reference
- https://dev.to/pr0pm/run-postgresql-pgadmin-in-pods-using-podman-386o
- https://mehmetozanguven.com/run-postgresql-with-podman/
