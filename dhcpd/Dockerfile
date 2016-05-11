FROM ubuntu:trusty

RUN apt-get -q -y update \
    && apt-get -q -y -o "DPkg::Options::=--force-confold" -o "DPkg::Options::=--force-confdef" dist-upgrade

RUN apt-get -q -y -o "DPkg::Options::=--force-confold" -o "DPkg::Options::=--force-confdef" install isc-dhcp-server \
    && apt-get -q -y autoremove \
    && apt-get -q -y clean \
    && rm -rf /var/lib/apt/lists/*

ADD util/my_init.py /sbin/my_init
ADD util/entrypoint.sh /entrypoint.sh

