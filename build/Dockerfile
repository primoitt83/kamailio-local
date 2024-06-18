FROM ghcr.io/florian-h05/webrtc-sip-gw:latest

RUN \
    apt update && \
    apt install -y kamailio-websocket-modules \
        kamailio-postgres-modules kamailio-presence-modules kamailio-outbound-modules \
        kamailio-utils-modules kamailio-json-modules kamailio-tls-modules \
        kamailio-geoip2-modules kamailio-extra-modules sngrep vim net-tools telnet tcpdump openssl

# Cleanup.
RUN apt-get clean autoclean
RUN apt-get autoremove --yes
RUN rm -rf /var/cache/* /tmp/* /var/lib/{apt,dpkg,cache,log}/    

## files for kamailio
#RUN mkdir -p /etc/certs

COPY ${PWD}/kamailio.cfg /etc/kamailio/kamailio.cfg
COPY ${PWD}/config.cfg /etc/kamailio/config.cfg
#COPY ${PWD}/kamailio-selfsigned.key /etc/certs/kamailio-selfsigned.key
#COPY ${PWD}/kamailio-selfsigned.pem /etc/certs/kamailio-selfsigned.pem
COPY ${PWD}/tls.cfg /etc/kamailio/tls.cfg
COPY ${PWD}/kamctlrc /etc/kamailio/kamctlrc
COPY ${PWD}/tabelas_view.sql /etc/kamailio/tabelas_view.sql
COPY ${PWD}/kss /usr/bin/kss

## files for rtpengine
COPY ${PWD}/rtpengine.conf /etc/rtpengine/rtpengine.conf

## copy new entrypoint.sh
COPY ${PWD}/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
RUN chmod +x /healthcheck.sh
RUN chmod +x /usr/bin/kss

EXPOSE 5060 5060/udp 8080 5061 5090/udp 8443
EXPOSE 18400-18500/udp

ENTRYPOINT ["/entrypoint.sh"]
CMD ["/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf -u root"]