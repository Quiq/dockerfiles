# dockerfiles

alpine version based on https://github.com/anapsix/docker-alpine-java with these changes:
* all downloads with https
* sha256sum check all downloads
* use alpine 3.5
* use en_US.UTF-8 lang and create correctly with localedef
* install dumb-init and configure as entrypoint
* install python, py-requests and openssl
