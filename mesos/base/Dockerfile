# ADAPTED FROM https://github.com/apache/mesos/blob/master/Dockerfile
# Dockerfile for building Mesos from source on raspberry pi
# 
# Create snapshot builds with:
# docker build -t mesos/mesos:git-`git rev-parse --short HEAD` .
#
# Run master/slave with:
# docker run mesos/mesos:git-`git rev-parse --short HEAD` mesos-master [options]
# docker run mesos/mesos:git-`git rev-parse --short HEAD` mesos-slave [options]
#
FROM hominidae/armhf-wheezy
MAINTAINER Adam Ossenford <AdamOssenford@gmail.com>

##################################################################
# APT SOURCES HACK IS MOST LIKELY ALADEEN
##################################################################
RUN gpg --keyserver pgpkeys.mit.edu --recv-key 9165938D90FDDD2E
RUN gpg -a --export 9165938D90FDDD2E | sudo apt-key add -
RUN gpg --keyserver pgpkeys.mit.edu --recv-key 7638D0442B90D010
RUN gpg -a --export 7638D0442B90D010 | sudo apt-key add -
RUN echo "deb http://mirrordirector.raspbian.org/raspbian/ wheezy main contrib non-free rpi" >> /etc/apt/sources.list
RUN echo "deb http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi" >> /etc/apt/sources.list
RUN echo "deb-src http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi" >> /etc/apt/sources.list
RUN echo "deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi" >> /etc/apt/sources.list
RUN echo "deb http://archive.raspbian.org/raspbian jessie main contrib non-free rpi" >> /etc/apt/sources.list
RUN echo "deb-src http://archive.raspbian.org/raspbian jessie main contrib non-free rpi" >> /etc/apt/sources.list

# build packages
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update
RUN apt-get install -yq build-essential \
                        autoconf \
                        libtool \
                        zlib1g-dev \
 			git \
			dh-autoreconf \
 			libcurl4-nss-dev \
 			libsasl2-dev \
			openjdk-7-jdk \
			openjdk-7-jre-lib \
			maven \
			python-dev \
			python-boto \
			libsvn-dev \
			libapr1-dev \
			gettext-base \
			gettext

#############################################
# HACK FOR gcc 4.8 DEPENDENCIES
#############################################
RUN apt-get install -yq -t jessie gcc-4.8 g++-4.8
#RUN update-alternatives --remove-all gcc
#RUN update-alternatives --remove-all g++
#RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 20
#RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
#RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
#RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50



# export environment
ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-armhf
ENV JAVA_LDFLAGS="-L/usr/lib/jvm/jdk-7-openjdk-armhf/jre/lib/arm/server \
 -R/usr/lib/jvm/jdk-7-openjdk-armhf/jre/lib/arm/server -Wl,-ljvm"
ENV JAVA_CPPFLAGS="-I/usr/lib/jvm/jdk-7-openjdk-armhf/include \
 -I/usr/lib/jvm/jdk-7-openjdk-armhf/include/linux"
ENV CFLAGS="-O2 -pipe -march=armv6j -mtune=arm1176jzf-s -mfpu=vfp \
 -mfloat-abi=hard"
ENV CXXFLAGS="-O2 -pipe -march=armv6j -mtune=arm1176jzf-s -mfpu=vfp \
 -mfloat-abi=hard"
# include libmesos on library path
ENV LD_LIBRARY_PATH /usr/local/lib

# copy local checkout into /opt
ADD . /opt

WORKDIR /opt

####################################
# PULL DOWN AND BUILD IT
####################################
RUN git clone https://github.com/AdamOssenford/mesos.git /opt/build/mesos
RUN cd /opt/build/mesos
# configure
WORKDIR /opt/build/mesos
RUN ./bootstrap

# NOT SURE ABOUT THIS PART YET
RUN mkdir build && cd build && ../configure

WORKDIR /opt/build/mesos/build

# build and cleanup in a single layer
RUN make -j2 install #&& cd / && rm -rf /opt
