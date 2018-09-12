# dockerfiles

alpine version based on https://github.com/anapsix/docker-alpine-java with these changes:
* all downloads with https
* remove apk --allow-untrusted and install the gpg public key for glibc
* sha256sum check all downloads
* use en_US.UTF-8 lang and create correctly with localedef
* install dumb-init and configure as entrypoint
* install honest-profiler
