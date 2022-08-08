FROM debian:bullseye

ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="false"
ARG ENABLE_NONROOT_DOCKER="true"
ARG USE_MOBY="true"
ARG CLI_VERSION="latest"

ENV DOCKER_BUILDKIT=1

ARG USERNAME=automatic
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN export DEBIAN_FRONTEND=noninteractive

COPY library-scripts/*.sh /tmp/library-scripts/

RUN apt-get update \
    && /bin/bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true" \
    && /bin/bash /tmp/library-scripts/docker-debian.sh "${ENABLE_NONROOT_DOCKER}" "/var/run/docker-host.sock" "/var/run/docker.sock" "${USERNAME}" "${USE_MOBY}" "${CLI_VERSION}" \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts/

ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]

COPY ./.ssh /home/vscode/.ssh
RUN chown -R vscode:vscode /home/vscode/.ssh
RUN chmod 400 /home/vscode/.ssh/**
COPY ./.aws /home/vscode/.aws
RUN chown -R vscode:vscode /home/vscode/.aws
RUN chmod 400 /home/vscode/.aws/**

RUN mkdir -p /home/vscode/.vscode-server/extensions \
    /home/vscode/.vscode-server-insiders/extensions \
    && chown -R vscode \
    /home/vscode/.vscode-server \
    /home/vscode/.vscode-server-insiders

RUN apt-get -y install --no-install-recommends \
    jq