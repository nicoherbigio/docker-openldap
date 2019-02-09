FROM nicoherbigio/debian:stretch-slim

LABEL maintainer="Nico Herbig <nico@nicoherbig.io>"


# Add OpenLDAP user and group to ensure that the IDs (UID and GID)
# are consistently assigned regardless of which additional dependencies are added.

ENV LDAP_USER_ID=901 LDAP_USER=openldap
ENV LDAP_GROUP_ID=901 LDAP_GROUP=openldap

RUN set -ex \
    && groupadd -r -g ${LDAP_GROUP_ID} ${LDAP_GROUP} \
    && useradd -r -g ${LDAP_GROUP} -u ${LDAP_USER_ID} ${LDAP_USER}


# Install OpenLDAP

RUN set -ex \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends gnutls-bin ldap-utils openssl slapd ssl-cert \
    && rm -rf /etc/ldap/slapd.d /var/lib/ldap \
    && rm -rf /var/lib/apt/lists/*


# Configure OpenLDAP

COPY assets/etc /usr/local/etc/ldap/
COPY assets/docker-entrypoint /usr/local/bin/

RUN set -ex \
    && chmod +x /usr/local/bin/docker-entrypoint \
    && ln -s usr/local/bin/docker-entrypoint /

VOLUME /etc/ldap/slapd.d
VOLUME /var/lib/ldap

EXPOSE 389 636

ENTRYPOINT ["/docker-entrypoint"]
