FROM debian:stable

RUN apt-get update
RUN apt-get install -y nfs-ganesha nfs-ganesha-vfs
RUN mkdir -p /var/run/dbus

EXPOSE 111/udp 111 2049 38465-38467 662

ADD start-nfs-ganesha.sh /

CMD ["/start-nfs-ganesha.sh"]
