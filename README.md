# dockerfiles

alpine version based on https://github.com/anapsix/docker-alpine-java with these changes:
* all downloads with https
* sha256sum check all downloads
* use alpine 3.6
* use en_US.UTF-8 lang and create correctly with localedef
* install dumb-init and configure as entrypoint
* install python3, py3-requests
* install honest-profiler
* upgrade to glibc 2.25
