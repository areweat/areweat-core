# Local Testing

## Fluvio

Deploy a local cluster, e.g. with  minikube / k3d etc.
- https://github.com/infinyon/fluvio/blob/master/DEVELOPER.md

## Mail Server

Either go Postfix+Dovecot C-stack route or Rust via Stalwart which is Rust-stack mail server.

### Stalwart Bootstrap

- Docker @ https://stalw.art/docs/install/docker/

$ docker pull stalwartlabs/mail-server:latest

$ mkdir -p ~/areweat-stalwart

$ docker run -d -ti \
             -p 25:25 \
             -p 443:443 \
             -p 587:587 \
             -p 465:465 \
             -p 143:143 \
             -p 993:993 \
             -p 4190:4190 \
             -v ~/areweat-stalwart:/opt/stalwart-mail \
             --name areweat-stalwart stalwartlabs/mail-server

$ docker exec -it areweat-stalwart /bin/sh /usr/local/bin/configure.sh

Given the IMAP is separate "black box" just go with RocksDB in-container

If you have a real domain provision the DMARC/DKIM/SPF DNS so the inbox appears as verified.

To provision needed account e.g. in stalwart:

docker exec -it areweat-stalwart /bin/sh -c 'usr/local/bin/stalwart-cli -u https://localhost:443 -c admin:{PASS} {COMMAND}'

$ domain create test.domain

$ account create test-account test-account-password

$ account add-email test-account test@email.domain

$ account display test-account

### Debug SMTP/IMAP

$ nc -v localhost 25

Note stalwart is fussy about <CR><LF> which in unix xterm typically are ^M^J given Enter is just regular <LF> w/o <CR>

### Debug IMAP

- https://www.atmail.com/blog/imap-101-manual-imap-sessions/

$ openssl s_client -connect localhost:143 -crlf -quiet -starttls imap
