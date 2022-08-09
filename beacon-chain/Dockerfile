ARG UPSTREAM_VERSION
FROM consensys/teku:$UPSTREAM_VERSION

USER root
RUN  apt-get update \
    && apt-get install -y wget \
    && rm -rf /var/lib/apt/lists/* \
    && wget -q https://raw.githubusercontent.com/gnosischain/teku-client/main/chiado/config.yaml -O  /usr/config.yaml && \
    wget -q https://github.com/gnosischain/teku-client/blob/main/chiado/genesis.ssz?raw=true -O /usr/genesis.ssz

########
# TEKU #
########
# IMPORTANT!: this docker image has a root user with password. Cannot execute apt update and apt install

# Give proper permissions to docker volume https://docs.teku.consensys.net/en/latest/HowTo/Get-Started/Installation-Options/Run-Docker-Image/#allow-multiple-users-to-run-the-docker-image

RUN mkdir -p /opt/teku/data
RUN chown -R 1001:1001 /opt/teku/data

ENV JAVA_OPTS="-Xmx4g"

COPY entrypoint.sh /usr/bin/entrypoint.sh
COPY jwtsecret.hex /jwtsecret
RUN chmod +x /usr/bin/entrypoint.sh

# API port: https://docs.teku.consensys.net/en/latest/Reference/CLI/CLI-Syntax/#rest-api-port
EXPOSE $BEACON_API_PORT
# P2P port: https://docs.teku.consensys.net/en/latest/Reference/CLI/CLI-Syntax/#p2p-port
#EXPOSE 9000

ENTRYPOINT [ "entrypoint.sh" ]    