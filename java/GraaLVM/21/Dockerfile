FROM --platform=$TARGETOS/$TARGETARCH ghcr.io/graalvm/jdk-community:21-ol8

RUN microdnf update \
    && microdnf install -y curl ca-certificates openssl git tar sqlite \
        fontconfig tzdata iproute gcc gcc-c++ freetype libstdc++ lsof \
        glibc-locale-source glibc-langpack-en tini \
    && microdnf clean all \
    && useradd -m -d /home/container -s /bin/bash container

ENV LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --from=ghcr.io/pelican-eggs/yolks:java_21 /entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]