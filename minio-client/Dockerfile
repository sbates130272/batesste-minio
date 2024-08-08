FROM ubuntu:latest

RUN apt-get update && \
  apt-get upgrade -y \
  s3fs \
  fio \
  wget

RUN mkdir -p /mnt/batesste-minio
COPY .passwd-s3fs /root/
