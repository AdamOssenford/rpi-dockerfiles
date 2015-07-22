FROM hypriot/rpi-alpine-scratch
MAINTAINER Adam Ossenford <AdamOssenford@gmail.com>
RUN apk update && apk upgrade && apk add bash && rm -rf /var/cache/apk/*

ENTRYPOINT ["/bin/ping"]
