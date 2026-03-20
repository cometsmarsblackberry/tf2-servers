ARG REGISTRY=ghcr.io
ARG OWNER=melkortf
ARG TF2_PLUGINS_BASE_TAG=latest

FROM ubuntu:22.04 AS plugins
SHELL ["/bin/bash", "-c"]
RUN export DEBIAN_FRONTEND=noninteractive \
    && export TZ=Etc/UTC \
    && apt-get -y update \
    && apt-get install -y --no-install-recommends --no-install-suggests \
    unzip \
    wget \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /download
ARG PLUGIN_INSTALL_DIR=/server/tf
RUN mkdir -p "${PLUGIN_INSTALL_DIR}"

# Arena Waiting For Players (enables WFP in arena mode)
ARG ARENAWFP_VERSION=1.0.0
ARG ARENAWFP_FILE_NAME=arenawfp.zip
ARG ARENAWFP_URL=https://github.com/Mikusch/arenawfp/releases/download/${ARENAWFP_VERSION}/${ARENAWFP_FILE_NAME}
ADD ${ARENAWFP_URL} .
RUN mkdir -p "${PLUGIN_INSTALL_DIR}/addons/sourcemod/" \
    && unzip -q -o "${ARENAWFP_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# Connect plugin (safer OnClientPreConnect forward - extension comes with STAC)
ARG CONNECT_VERSION=1.4.0-git101-8d83d5e
ARG CONNECT_FILE_NAME=connect-${CONNECT_VERSION}-linux.zip
ARG CONNECT_URL=https://builds.limetech.io/files/${CONNECT_FILE_NAME}
ADD ${CONNECT_URL} .
RUN unzip -q -o "${CONNECT_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# NativeVotes (extension + plugins for native game voting UI)
ARG NATIVEVOTES_VERSION=sm_1.12
ARG NATIVEVOTES_FILE_NAME=nativevotes_${NATIVEVOTES_VERSION}.zip
ARG NATIVEVOTES_URL=https://github.com/sapphonie/sourcemod-nativevotes-updated/releases/latest/download/${NATIVEVOTES_FILE_NAME}
ADD ${NATIVEVOTES_URL} .
RUN unzip -q -o "${NATIVEVOTES_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/" \
    && rm -rf "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/disabled" \
    && rm -rf "${PLUGIN_INSTALL_DIR}/addons/sourcemod/scripting"

# ============================================================================
# Final stage: Combine everything
# ============================================================================
FROM ${REGISTRY}/${OWNER}/tf2-plugins-base/i386:${TF2_PLUGINS_BASE_TAG}
LABEL maintainer="garrappachc@gmail.com"

ARG PLUGIN_INSTALL_DIR=/server/tf
COPY --from=plugins --chown=tf2 "${PLUGIN_INSTALL_DIR}/" "${SERVER_DIR}/tf/"
