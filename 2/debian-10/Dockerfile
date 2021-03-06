FROM docker.io/bitnami/minideb:buster
LABEL maintainer "TPayMobile <BigData@tpaymobile.com>"

ENV BITNAMI_PKG_CHMOD="-R a+rwX" \
    BITNAMI_PKG_EXTRA_DIRS="/opt/bitnami/airflow/dags" \
    HOME="/" \
    PATH="/opt/bitnami/python/bin:/opt/bitnami/postgresql/bin:/opt/bitnami/common/bin:/opt/bitnami/airflow/venv/bin:/opt/bitnami/nami/bin:/opt/bitnami/java/bin:$PATH" \
    JAVA_HOME="/opt/bitnami/java" 

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip libbsd0 libbz2-1.0 libc6 libcom-err2 libedit2 libffi6 libgcc1 libgmp10 libgnutls30 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libmariadb3 libncursesw6 libnettle6 libp11-kit0 libreadline7 libsasl2-2 libsqlite3-0 libssl1.1 libstdc++6 libtasn1-6 libtinfo6 libunistring2 libuuid1 libxml2 libxslt1.1 locales netbase procps sudo tar zlib1g wget unzip
RUN /build/bitnami-user.sh
RUN /build/install-nami.sh
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/java-1.8.232-0-linux-amd64-debian-9.tar.gz && \
    echo "f4e5ff1d4fb13baf9bb2a4f2e5ffb328cf9eeb26d0b91214196e4473c2874adc  /tmp/bitnami/pkg/cache/java-1.8.232-0-linux-amd64-debian-9.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/java-1.8.232-0-linux-amd64-debian-9.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/java-1.8.232-0-linux-amd64-debian-9.tar.gz
RUN bitnami-pkg install python-3.8.8-0 --checksum e41a0265dbeaf92e3e04bd6e42dffc7d18139a829a49f11eeebb107eb32a17cc
RUN bitnami-pkg install postgresql-client-10.16.0-0 --checksum 6ae2df74c4cc145690104c9bfbd4f9977cc00d26b3a010bb1eba74d92048485d
RUN bitnami-pkg install tini-0.19.0-1 --checksum 9b1f1c095944bac88a62c1b63f3bff1bb123aa7ccd371c908c0e5b41cec2528d
RUN bitnami-pkg install gosu-1.12.0-2 --checksum 4d858ac600c38af8de454c27b7f65c0074ec3069880cb16d259a6e40a46bbc50
RUN bitnami-pkg unpack airflow-2.0.1-1 --checksum 6f3f48e9b3717df7140abd7e007323682fd0a9c91c27ace5fbe0f38b6f2d2397
RUN localedef -c -f UTF-8 -i en_US en_US.UTF-8
RUN update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX && \
    DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales
RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && locale-gen
RUN /build/bitnami-user.sh


COPY rootfs /
RUN /opt/bitnami/scripts/locales/add-extra-locales.sh
ENV AIRFLOW_BASE_URL="" \
    AIRFLOW_DATABASE_HOST="postgresql" \
    AIRFLOW_DATABASE_NAME="bitnami_airflow" \
    AIRFLOW_DATABASE_PASSWORD="bitnami1" \
    AIRFLOW_DATABASE_PORT_NUMBER="5432" \
    AIRFLOW_DATABASE_USERNAME="bn_airflow" \
    AIRFLOW_DATABASE_USE_SSL="no" \
    AIRFLOW_EMAIL="user@example.com" \
    AIRFLOW_EXECUTOR="SequentialExecutor" \
    AIRFLOW_FERNET_KEY="" \
    AIRFLOW_FIRSTNAME="Firstname" \
    AIRFLOW_HOME="/opt/bitnami/airflow" \
    AIRFLOW_HOSTNAME_CALLABLE="" \
    AIRFLOW_LASTNAME="Lastname" \
    AIRFLOW_LDAP_ALLOW_SELF_SIGNED="True" \
    AIRFLOW_LDAP_BIND_PASSWORD="" \
    AIRFLOW_LDAP_BIND_USER="" \
    AIRFLOW_LDAP_ENABLE="no" \
    AIRFLOW_LDAP_SEARCH="" \
    AIRFLOW_LDAP_TLS_CA_CERTIFICATE="" \
    AIRFLOW_LDAP_UID_FIELD="uid" \
    AIRFLOW_LDAP_URI="" \
    AIRFLOW_LDAP_USE_TLS="" \
    AIRFLOW_LOAD_EXAMPLES="yes" \
    AIRFLOW_PASSWORD="bitnami" \
    AIRFLOW_POOL_DESC="" \
    AIRFLOW_POOL_NAME="" \
    AIRFLOW_POOL_SIZE="" \
    AIRFLOW_REDIS_USE_SSL="no" \
    AIRFLOW_USERNAME="user" \
    AIRFLOW_USER_REGISTRATION_ROLE="Public" \
    AIRFLOW_WEBSERVER_HOST="127.0.0.1" \
    AIRFLOW_WEBSERVER_PORT_NUMBER="8080" \
    BITNAMI_APP_NAME="airflow" \
    BITNAMI_IMAGE_VERSION="2.0.1-debian-10-r15" \
    LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LD_LIBRARY_PATH="/opt/bitnami/python/lib/:/opt/bitnami/airflow/venv/lib/python3.8/site-packages/numpy.libs/:$LD_LIBRARY_PATH" \
    LIBNSS_WRAPPER_PATH="/opt/bitnami/common/lib/libnss_wrapper.so" \
    LNAME="airflow" \
    NAMI_PREFIX="/.nami" \
    NSS_WRAPPER_GROUP="/opt/bitnami/airflow/nss_group" \
    NSS_WRAPPER_PASSWD="/opt/bitnami/airflow/nss_passwd" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux" \
    REDIS_HOST="redis" \
    REDIS_PASSWORD="" \
    REDIS_PORT_NUMBER="6379" \
    REDIS_USER="" 

EXPOSE 8080

USER 1001
ENTRYPOINT [ "/app-entrypoint.sh" ]
CMD [ "/run.sh" ]
