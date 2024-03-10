# Local Testing

## Mail Server

Either go Postfix+Dovecot route or Rust via Stalwart

### Stalwart Bootstrap

- Docker @ https://stalw.art/docs/install/docker/

$ docker pull stalwartlabs/mail-server:latest

$ mkdir -p ~/areweat-stalwart

$ docker run -d -ti -p 8443:8443 \
             -p 8825:25 \
             -p 443:443 \
             -p 8587:587 \
             -p 8465:465 \
             -p 8143:143 \
             -p 8993:993 \
             -p 4190:4190 \
             -v ~/areweat-stalwart:/opt/stalwart-mail \
             --name areweat-stalwart stalwartlabs/mail-server

$ docker exec -it areweat-stalwart /bin/sh /usr/local/bin/configure.sh

Given the IMAP is separate "black box" just go with RocksDB in-container
