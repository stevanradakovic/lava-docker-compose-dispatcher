ARG version=latest
FROM lavasoftware/lava-dispatcher:${version}

ARG extra_packages=""
RUN apt -q update || apt -q update
RUN DEBIAN_FRONTEND=noninteractive apt-get -q -y install software-properties-common nfs-common
RUN apt-add-repository non-free
RUN apt -q update || apt -q update
RUN DEBIAN_FRONTEND=noninteractive apt-get -q -y install ${extra_packages} net-tools snmp snmp-mibs-downloader
RUN download-mibs

# Add MIBs
RUN mkdir -p /usr/share/snmp/mibs/
ADD powernet428.mib /usr/share/snmp/mibs/

# Add certificates.
COPY certs/* /etc/lava-dispatcher/certificates.d/

# Add lab scripts
RUN mkdir -p /usr/local/lab-scripts/
ADD https://git.linaro.org/lava/lava-lab.git/plain/shared/lab-scripts/snmp_pdu_control /usr/local/lab-scripts/
RUN chmod a+x /usr/local/lab-scripts/snmp_pdu_control
ADD https://git.linaro.org/lava/lava-lab.git/plain/shared/lab-scripts/eth008_control /usr/local/lab-scripts/
RUN chmod a+x /usr/local/lab-scripts/eth008_control

#ARG master_url
#ARG logger_url
#ARG encrypt=""
#RUN echo "MASTER_URL=\"${master_url}\"" >> /etc/lava-dispatcher/lava-slave
#RUN echo "LOGGER_URL=\"${logger_url}\"" >> /etc/lava-dispatcher/lava-slave
#RUN echo "ENCRYPT=\"${encrypt}\"" >> /etc/lava-dispatcher/lava-slave

#ENV LIBGUESTFS_BACKEND=direct

EXPOSE 2049

ENTRYPOINT ["/root/entrypoint.sh"]
