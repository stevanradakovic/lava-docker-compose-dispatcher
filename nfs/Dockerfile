FROM debian:stable

RUN apt-get update
RUN apt-get install -y nfs-common nfs-ganesha nfs-ganesha-vfs

ADD start-nfs-ganesha.sh /

CMD ["/start-nfs-ganesha.sh"]
