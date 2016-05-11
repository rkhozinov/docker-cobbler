FROM ubuntu:trusty

RUN apt-get -q -y update \
    && apt-get -q -y -o "DPkg::Options::=--force-confold" -o "DPkg::Options::=--force-confdef" dist-upgrade

RUN apt-get -q -y -o "DPkg::Options::=--force-confold" -o "DPkg::Options::=--force-confdef" install cobbler -cobbler-web supervisor \
    && apt-get -q -y autoremove \
    && apt-get -q -y clean \
    && rm -rf /var/lib/apt/lists/*

ADD conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
#ADD conf/dnsmasq.template /etc/cobbler/dnsmasq.template
RUN (echo -n "cobbler:Cobbler:" && echo -n "cobbler:Cobbler:cobbler" | md5sum | awk '{print $1}') > /etc/cobbler/users.digest

#RUN sed -i "s/^manage_dhcp: .*$/manage_dhcp: 1/" /etc/cobbler/settings
#RUN sed -i "s/^manage_dns: .*$/manage_dns: 1/" /etc/cobbler/settings
#RUN sed -i "s/^module = manage_bind/module = manage_dnsmasq/" /etc/cobbler/modules.conf
#RUN sed -i "s/^module = manage_isc/module = manage_dnsmasq/" /etc/cobbler/modules.conf

ADD init-cobbler /usr/local/bin/init-cobbler
CMD ["init-cobbler"]
ADD http://cz.archive.ubuntu.com/ubuntu/dists/trusty/main/installer-amd64/current/images/netboot/mini.iso /usr
CMD cd /usr
CMD mount -o loop mini.iso /mnt
CMD cobbler import --name=ubuntu-14.04 --path=/mnt
CMD umount /mnt
 
CMD ["init-cobbler"]
CMD /bin/bash
