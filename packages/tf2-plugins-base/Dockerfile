ARG REGISTRY=ghcr.io
ARG OWNER=melkortf
ARG TF2_SOURCEMOD_TAG=latest

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

# SourceBans++
ARG SOURCEBANS_VERSION=1.7.0
ARG SOURCEBANS_FILE_NAME=sourcebans-pp-${SOURCEBANS_VERSION}.plugins-only.tar.gz
ARG SOURCEBANS_URL=https://github.com/sbpp/sourcebans-pp/releases/download/${SOURCEBANS_VERSION}/${SOURCEBANS_FILE_NAME}
ARG SOURCEBANS_CHECKSUM=07ac3d745d544a8b93d2ccad148edcaea802960982d9776b4bea0eea5e792bd4
ADD --checksum=sha256:${SOURCEBANS_CHECKSUM} ${SOURCEBANS_URL} .
RUN tar xf "${SOURCEBANS_FILE_NAME}" -C "${PLUGIN_INSTALL_DIR}/"

# tf2-comp-fixes
ARG COMP_FIXES_FILE_NAME=tf2-comp-fixes.zip
ARG COMP_FIXES_VERSION=1.16.19
ARG COMP_FIXES_URL=https://github.com/ldesgoui/tf2-comp-fixes/releases/download/v${COMP_FIXES_VERSION}/${COMP_FIXES_FILE_NAME}
ARG COMP_FIXES_CHECKSUM=4a54bdafda9d8f462ac5bfcc9d402840f1954dbfacdb5d17a06e20b796e760e1
ADD --checksum=sha256:${COMP_FIXES_CHECKSUM} ${COMP_FIXES_URL} .
RUN unzip -q -o "${COMP_FIXES_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# SM_Neocurl
ARG NEOCURL_FILE_NAME=sm-neocurl-repack.zip
ARG NEOCURL_VERSION=v2.0.1-beta1
ARG NEOCURL_URL=https://github.com/sapphonie/SM-neocurl-ext/releases/download/${NEOCURL_VERSION}/${NEOCURL_FILE_NAME}
ARG NEOCURL_CHECKSUM=cddbef00ca5b4b2a14c35fd062fe18586fb42f68c6d6366bf74f5b72a5734e44
ADD --checksum=sha256:${NEOCURL_CHECKSUM} ${NEOCURL_URL} .
RUN unzip -q -o "${NEOCURL_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# STAC Anti-Cheat
ARG STAC_FILE_NAME=stac.zip
ARG STAC_VERSION=v6.3.8
ARG STAC_URL=https://github.com/sapphonie/StAC-tf2/releases/download/${STAC_VERSION}/${STAC_FILE_NAME}
ARG STAC_CHECKSUM=ec64fc4f697754038526591cc8ef6a99d331b5e962eac36d968dfb25c0c69957
ADD --checksum=sha256:${STAC_CHECKSUM} ${STAC_URL} .
RUN unzip -q -o "${STAC_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# sm-ripext (REST in Pawn Extension)
ARG RIPEXT_FILE_NAME=sm-ripext-1.3.2-linux.zip
ARG RIPEXT_VERSION=1.3.2
ARG RIPEXT_URL=https://github.com/ErikMinekus/sm-ripext/releases/download/${RIPEXT_VERSION}/${RIPEXT_FILE_NAME}
ARG RIPEXT_CHECKSUM=c3afa6173b2d5210110e139b00cfb03f04c5615f44840d83be46aa60fc497624
ADD --checksum=sha256:${RIPEXT_CHECKSUM} ${RIPEXT_URL} .
RUN unzip -q -o "${RIPEXT_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# SteamWorks extension
ARG STEAMWORKS_VERSION=1.2.3c
ARG STEAMWORKS_FILE_NAME=package-lin.tgz
ARG STEAMWORKS_URL=https://github.com/KyleSanderson/SteamWorks/releases/download/${STEAMWORKS_VERSION}/${STEAMWORKS_FILE_NAME}
ADD ${STEAMWORKS_URL} .
RUN tar xzf "${STEAMWORKS_FILE_NAME}" -C "${PLUGIN_INSTALL_DIR}/"

# System2 extension (DISABLED - upstream legacy.inc has syntax incompatibility with SM 1.12)
# ARG SYSTEM2_VERSION=v3.3.2
# ARG SYSTEM2_FILE_NAME=system2.zip
# ARG SYSTEM2_URL=https://github.com/dordnung/System2/releases/download/${SYSTEM2_VERSION}/${SYSTEM2_FILE_NAME}
# ADD ${SYSTEM2_URL} .
# RUN unzip -q -o "${SYSTEM2_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# Map Downloader (pinned to commit)
ARG MAPDOWNLOADER_COMMIT=729db2ea9e7f353ca67d2c7c19e1cd1dc473928c
ARG MAPDOWNLOADER_PLUGIN_FILE_NAME=mapdownloader.smx
ARG MAPDOWNLOADER_PLUGIN_URL=https://github.com/spiretf/mapdownloader/raw/${MAPDOWNLOADER_COMMIT}/plugin/${MAPDOWNLOADER_PLUGIN_FILE_NAME}
ARG MAPDOWNLOADER_PLUGIN_CHECKSUM=53d4a64d40a939837dd3dfbf41aa3e682c421da44efb4f16c3db397aa2901ed2
ADD --checksum=sha256:${MAPDOWNLOADER_PLUGIN_CHECKSUM} ${MAPDOWNLOADER_PLUGIN_URL} .
RUN cp "${MAPDOWNLOADER_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${MAPDOWNLOADER_PLUGIN_FILE_NAME}"

# SM-TickrateChanger (release zip)
ARG TICKRATE_CHANGER_VERSION=1.1.1
ARG TICKRATE_CHANGER_FILE_NAME=SM-TickrateChanger.zip
ARG TICKRATE_CHANGER_URL=https://github.com/Mikusch/SM-TickrateChanger/releases/download/${TICKRATE_CHANGER_VERSION}/${TICKRATE_CHANGER_FILE_NAME}
ARG TICKRATE_CHANGER_CHECKSUM=8430b45fd961f18c53131c44f879f8effb691a8213775f5b3ae1a85b2e0dfe34
ADD --checksum=sha256:${TICKRATE_CHANGER_CHECKSUM} ${TICKRATE_CHANGER_URL} .
RUN unzip -q -o "${TICKRATE_CHANGER_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# TF2-Imageboard-GreenText (release smx)
ARG GREENTEXT_VERSION=1.0.0
ARG GREENTEXT_FILE_NAME=tf2_4chan_quoter.smx
ARG GREENTEXT_URL=https://github.com/KohtepSP/TF2-Imageboard-GreenText/releases/download/${GREENTEXT_VERSION}/${GREENTEXT_FILE_NAME}
ADD ${GREENTEXT_URL} "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${GREENTEXT_FILE_NAME}"

# funcommandsX (precompiled with translations and config)
ARG FUNCOMMANDSX_COMMIT=main
ARG FUNCOMMANDSX_BASE_URL=https://raw.githubusercontent.com/Dosergen/Stuff/${FUNCOMMANDSX_COMMIT}/funcommandsX
ADD ${FUNCOMMANDSX_BASE_URL}/plugins/funcommandsX.smx "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/funcommandsX.smx"
ADD ${FUNCOMMANDSX_BASE_URL}/translations/funcommandsX.phrases.txt "${PLUGIN_INSTALL_DIR}/addons/sourcemod/translations/funcommandsX.phrases.txt"
ADD ${FUNCOMMANDSX_BASE_URL}/configs/happy_phrases.ini "${PLUGIN_INSTALL_DIR}/addons/sourcemod/configs/happy_phrases.ini"

# Reloadmap (precompiled)
ARG RELOADMAP_COMMIT=main
ARG RELOADMAP_URL=https://raw.githubusercontent.com/WidovV/Reloadmap/${RELOADMAP_COMMIT}/plugins/reloadmap.smx
ADD ${RELOADMAP_URL} "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/reloadmap.smx"

# TF2-Simple-FOV (precompiled)
ARG SIMPLE_FOV_COMMIT=main
ARG SIMPLE_FOV_URL=https://raw.githubusercontent.com/CallMeBlad3z/TF2-Simple-FOV/${SIMPLE_FOV_COMMIT}/plugins/simple-fov.smx
ADD ${SIMPLE_FOV_URL} "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/simple-fov.smx"

# ============================================================================
# Stage 2: Compile source-only plugins
# ============================================================================
FROM ubuntu:22.04 AS compiler
SHELL ["/bin/bash", "-c"]
RUN export DEBIAN_FRONTEND=noninteractive \
    && export TZ=Etc/UTC \
    && apt-get -y update \
    && apt-get install -y --no-install-recommends --no-install-suggests \
    wget \
    ca-certificates \
    lib32stdc++6 \
    lib32z1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /compile

# Download SourceMod for the compiler (use latest stable)
ARG SOURCEMOD_URL=https://sm.alliedmods.net/smdrop/1.12/sourcemod-1.12.0-git7221-linux.tar.gz
RUN wget -q "${SOURCEMOD_URL}" -O sourcemod.tar.gz \
    && mkdir -p /sourcemod \
    && tar xzf sourcemod.tar.gz -C /sourcemod \
    && chmod +x /sourcemod/addons/sourcemod/scripting/spcomp

# Set up scripting directory
RUN mkdir -p /compile/scripting/include /compile/plugins /compile/gamedata

# Copy default includes
RUN cp -r /sourcemod/addons/sourcemod/scripting/include/* /compile/scripting/include/

# Download additional include files for extensions
# SteamWorks include
ARG STEAMWORKS_INC_COMMIT=master
ADD https://raw.githubusercontent.com/KyleSanderson/SteamWorks/${STEAMWORKS_INC_COMMIT}/Pawn/includes/SteamWorks.inc /compile/scripting/include/SteamWorks.inc

# System2 include (DISABLED - upstream legacy.inc has syntax incompatibility with SM 1.12)
# ARG SYSTEM2_INC_COMMIT=master
# RUN mkdir -p /compile/scripting/include/system2
# ADD https://raw.githubusercontent.com/dordnung/System2/${SYSTEM2_INC_COMMIT}/sourcemod/scripting/include/system2.inc /compile/scripting/include/system2.inc
# ADD https://raw.githubusercontent.com/dordnung/System2/${SYSTEM2_INC_COMMIT}/sourcemod/scripting/include/system2/legacy.inc /compile/scripting/include/system2/legacy.inc
# ADD https://raw.githubusercontent.com/dordnung/System2/${SYSTEM2_INC_COMMIT}/sourcemod/scripting/include/system2/request.inc /compile/scripting/include/system2/request.inc


# Download source files for plugins that need compilation
# ChatExec
ARG CHATEXEC_COMMIT=main
ADD https://raw.githubusercontent.com/SNWCreations/ChatExec/${CHATEXEC_COMMIT}/scripting/chat_exec.sp /compile/scripting/chat_exec.sp

# Command_Buffer (source + gamedata)
ARG COMMAND_BUFFER_COMMIT=main
ADD https://raw.githubusercontent.com/SilvDev/Command_Buffer/${COMMAND_BUFFER_COMMIT}/scripting/command_buffer.sp /compile/scripting/command_buffer.sp
ADD https://raw.githubusercontent.com/SilvDev/Command_Buffer/${COMMAND_BUFFER_COMMIT}/gamedata/command_buffer.games.txt /compile/gamedata/command_buffer.games.txt

# HideViewmodel (regular version with clientprefs)
ARG HIDEVIEWMODEL_COMMIT=main
ADD https://raw.githubusercontent.com/notkoen/HideViewmodel/${HIDEVIEWMODEL_COMMIT}/HideViewmodel.sp /compile/scripting/HideViewmodel.sp

# linux_file_lowercaser (source + gamedata)
ARG LINUX_LOWERCASER_COMMIT=master
ADD https://raw.githubusercontent.com/rtldg/linux_file_lowercaser/${LINUX_LOWERCASER_COMMIT}/addons/sourcemod/scripting/linux_file_lowercaser.sp /compile/scripting/linux_file_lowercaser.sp
ADD https://raw.githubusercontent.com/rtldg/linux_file_lowercaser/${LINUX_LOWERCASER_COMMIT}/addons/sourcemod/gamedata/linux_file_lowercaser.txt /compile/gamedata/linux_file_lowercaser.txt

# noautokick and nospecmsg (from sapphonie/gameservers)
ARG SAPPHONIE_COMMIT=stable
ADD https://raw.githubusercontent.com/sapphonie/gameservers/${SAPPHONIE_COMMIT}/tf/addons/sourcemod/scripting/noautokick.sp /compile/scripting/noautokick.sp
ADD https://raw.githubusercontent.com/sapphonie/gameservers/${SAPPHONIE_COMMIT}/tf/addons/sourcemod/scripting/nospecmsg.sp /compile/scripting/nospecmsg.sp

# PlayerAnalytics (source + gamedata)
ARG PLAYERANALYTICS_COMMIT=master
ADD https://raw.githubusercontent.com/sneak-it/PlayerAnalytics/${PLAYERANALYTICS_COMMIT}/scripting/player_analytics.sp /compile/scripting/player_analytics.sp
ADD https://raw.githubusercontent.com/sneak-it/PlayerAnalytics/${PLAYERANALYTICS_COMMIT}/gamedata/detect_os.games.txt /compile/gamedata/detect_os.games.txt

# restartsoon (from sapphonie/gameservers)
# syskill DISABLED - requires System2 which has upstream syntax incompatibility
ARG SPACESHIP_COMMIT=master
ADD https://raw.githubusercontent.com/sapphonie/gameservers/${SAPPHONIE_COMMIT}/tf/addons/sourcemod/scripting/restartsoon.sp /compile/scripting/restartsoon.sp
# ADD https://raw.githubusercontent.com/sapphonie/gameservers/${SAPPHONIE_COMMIT}/tf/addons/sourcemod/scripting/syskill.sp /compile/scripting/syskill.sp

# Spray_Exploit
ARG SPRAY_EXPLOIT_COMMIT=main
ADD https://raw.githubusercontent.com/SilvDev/Spray_Exploit/${SPRAY_EXPLOIT_COMMIT}/scripting/spray_exploit_fixer.sp /compile/scripting/spray_exploit_fixer.sp

# webshortcuts (from castaway-plugins)
ARG CASTAWAY_COMMIT=master
ADD https://raw.githubusercontent.com/rsedxcftvgyhbujnkiqwe/castaway-plugins/${CASTAWAY_COMMIT}/scripting/webshortcuts.sp /compile/scripting/webshortcuts.sp

# arena-autoscramble (from castaway-plugins)
ADD https://raw.githubusercontent.com/rsedxcftvgyhbujnkiqwe/castaway-plugins/${CASTAWAY_COMMIT}/scripting/arena-autoscramble.sp /compile/scripting/arena-autoscramble.sp
ADD https://raw.githubusercontent.com/rsedxcftvgyhbujnkiqwe/castaway-plugins/${CASTAWAY_COMMIT}/scripting/include/scramble.inc /compile/scripting/include/scramble.inc

# Compile all source plugins
RUN cd /compile/scripting \
    && for sp_file in *.sp; do \
    echo "Compiling ${sp_file}..." \
    && /sourcemod/addons/sourcemod/scripting/spcomp \
    -i/compile/scripting/include \
    -i/sourcemod/addons/sourcemod/scripting/include \
    -o/compile/plugins/${sp_file%.sp}.smx \
    "${sp_file}" \
    || echo "Warning: Failed to compile ${sp_file}"; \
    done

# ============================================================================
# Final stage: Combine everything
# ============================================================================
FROM ${REGISTRY}/${OWNER}/tf2-sourcemod/i386:${TF2_SOURCEMOD_TAG}
LABEL maintainer="garrappachc@gmail.com"

ARG PLUGIN_INSTALL_DIR=/server/tf
COPY --from=plugins --chown=tf2 "${PLUGIN_INSTALL_DIR}/" "${SERVER_DIR}/tf/"

# Copy compiled plugins
COPY --from=compiler --chown=tf2 /compile/plugins/*.smx "${SERVER_DIR}/tf/addons/sourcemod/plugins/"

# Copy gamedata files
COPY --from=compiler --chown=tf2 /compile/gamedata/*.txt "${SERVER_DIR}/tf/addons/sourcemod/gamedata/"

# Disable useless plugins
RUN mkdir -p "${SERVER_DIR}/tf/addons/sourcemod/plugins/disabled" \
    && mv "${SERVER_DIR}/tf/addons/sourcemod/plugins/"{funcommands,funvotes}.smx "${SERVER_DIR}/tf/addons/sourcemod/plugins/disabled/"

ENV DOWNLOAD_URL=
ENV SM_MAP_DOWNLOAD_BASE=https://fastdl.serveme.tf/maps
