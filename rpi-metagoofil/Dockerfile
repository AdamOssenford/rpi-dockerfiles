FROM hypriot/rpi-alpine-scratch
MAINTAINER Adam Ossenford <AdamOssenford@gmail.com>
RUN apk update && apk upgrade && apk add bash && apk add git && apk add python && apk add py-pip && pip install --upgrade pip && pip install chardet &&  rm -rf /var/cache/apk/*
RUN git clone https://github.com/reverland/igt.git
WORKDIR /igt/metagoofil-blackhat
ENTRYPOINT ["/usr/bin/python", "metagoofil.py"]
