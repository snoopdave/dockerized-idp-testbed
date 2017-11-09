FROM debian:jessie

MAINTAINER Dave Johnson <snoopdave@gmail.com>

ENV OPENLDAP_VERSION 2.4.40
ENV SLAPD_PASSWORD password
ENV SLAPD_DOMAIN idptestbed
ENV SLAPD_ADDITIONAL_MODULES memberof

# loglevels  are here https://www.openldap.org/doc/admin24/slapdconfig.html
ENV DEBUG_LEVEL -1

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
        slapd=${OPENLDAP_VERSION}* && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mv /etc/ldap /etc/ldap.dist

COPY modules/ /etc/ldap.dist/modules
COPY prepopulate/ /etc/ldap.dist/prepopulate
COPY entrypoint.sh /entrypoint.sh

EXPOSE 389

VOLUME ["/etc/ldap", "/var/lib/ldap"]

ENTRYPOINT ["/entrypoint.sh"]

CMD ["sh", "-c", "slapd -d ${DEBUG_LEVEL} -u openldap -g openldap"]
