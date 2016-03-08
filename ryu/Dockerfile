# Ryu SDN Framework
#
# VERSION 0.0.1

FROM ubuntu:15.04

MAINTAINER FUJITA Tomonori <fujita.tomonori@lab.ntt.co.jp>

RUN apt-get update && apt-get install -y --no-install-recommends \
                        python-setuptools \
                        python-pip \
                        python-eventlet \
                        python-lxml \
                        python-msgpack \
                        unzip \
                        wget \
                   && rm -rf /var/lib/apt/lists/*

ENV HOME /root
WORKDIR /root

RUN wget --no-check-certificate https://github.com/mininet/mininet/archive/master.zip
RUN unzip master.zip && \
    cd mininet-master && \
    util/install.sh -n

RUN wget --no-check-certificate https://github.com/openvswitch/ovs/archive/master.zip
RUN unzip master.zip && \
    cd ovs-master && \
    apt-get install build-essential fakeroot autoconf libtool && \
    ./boot.sh && \
    ./configure --with-linux=/lib/modules/`uname -r`/build && \
    make && make install && make modules_install && depmod && \
    rm -f /usr/local/etc/openvswitch/conf.db /usr/local/etc/openvswitch/.conf.db.~lock~ && \
    ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema

RUN wget --no-check-certificate https://github.com/osrg/ryu/archive/master.zip
RUN unzip master.zip && \
    cd ryu-master && \
    pip install -r tools/pip-requires && \
    python setup.py install
