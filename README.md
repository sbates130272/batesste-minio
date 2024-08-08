# batesste-minio: A scratchpad for exploring object-store for AI

## Overview

This repository uses [MinIO][ref-minio] to explore using object
storage for AI workloads. It sets up a ephemeral MinIO server (single
instance in a container) and uses [s3fs][ref-s3fs] inside a client
container to mount the object storage so it can be accessed like a
filesystem. We then perform some fio testing against this.

## Initialization

To initialize the MinIO server for a fresh set of tests use something
like this:
```
docker compose --env-file .batesste-minio.env -f batesste-minio.yaml down -v && \
  docker compose --env-file .batesste-minio.env -f batesste-minio.yaml up -d --build --force-recreate
```
This will initialize a MinIO server and client, create access keys and
create an initial bucket.

You can then login to the client container using:
```
docker exec -it batesste-minio-minio-client-1 /bin/bash
```
Once inside the client container you can setup the s3fs mount point
using the following:
```
s3fs batesste-minio /mnt/batesste-minio \
  -o passwd_file=/root/.passwd-s3fs \
  -o url=http://minio-server:9000/ \
  -o use_path_request_style
```
You can then issue IO against the object-store, via the local s3fs
filesystem using something like:
```
dd if=/dev/random of=/mnt/minio-server/test-001.dat bs=1M count=1k
```

[ref-minio]: https://min.io/
[ref-s3fs]: https://github.com/s3fs-fuse/s3fs-fuse
