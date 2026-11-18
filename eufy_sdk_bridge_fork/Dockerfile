# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
#
# [fork] Builds the bridge FROM SOURCE on your Home Assistant host: no prebuilt image to publish or pull.
# It mirrors the bridge's own Dockerfile, but fetches the bridge from BRIDGE_REPO at the pinned
# BRIDGE_REF (set in build.yaml), installs the pinned beta SDK and applies the fork's NVR patch.
# Bump the add-on version after changing build.yaml so the Supervisor rebuilds.
ARG BUILD_FROM=node:24-alpine
FROM ${BUILD_FROM}

ARG BUILD_ARCH=amd64
ARG GO2RTC_VERSION=1.9.9
ARG BRIDGE_REPO=Nuvolares/ha-eufy-sdk-bridge
ARG BRIDGE_REF=main
ARG SDK_DIST_TAG=0.6.0-beta.72

# ffmpeg: the bridge's live feeds and go2rtc's on-demand H.264 twins. jq + curl: run.sh (options, discovery).
RUN apk add --no-cache ffmpeg curl jq

WORKDIR /app

RUN case "${BUILD_ARCH}" in \
      amd64) g2="go2rtc_linux_amd64" ;; \
      aarch64) g2="go2rtc_linux_arm64" ;; \
      *) echo "unsupported BUILD_ARCH: ${BUILD_ARCH}" && exit 1 ;; \
    esac \
 && curl -fsSL -o /usr/local/bin/go2rtc \
      "https://github.com/AlexxIT/go2rtc/releases/download/v${GO2RTC_VERSION}/${g2}" \
 && chmod +x /usr/local/bin/go2rtc

RUN echo "bridge: ${BRIDGE_REPO}@${BRIDGE_REF}, SDK: ${SDK_DIST_TAG}" \
 && curl -fsSL "https://codeload.github.com/${BRIDGE_REPO}/tar.gz/${BRIDGE_REF}" \
      | tar -xz --strip-components=1 -C /app \
 && npm ci --omit=dev --no-audit --no-fund \
 && npm install --omit=dev --no-audit --no-fund --no-save "@mega-yfue/eufy-sdk@${SDK_DIST_TAG}" \
 && node scripts/patch-sdk.mjs \
 && chmod +x bin/start.sh \
 && ln -sf /app/bin/start.sh /usr/local/bin/eufy-sdk-bridge \
 && mkdir -p /app/data

ENV BRIDGE_APP_DIR=/app BRIDGE_PORT=3000 BRIDGE_HOST=0.0.0.0 \
    GO2RTC_CONFIG=/app/data/go2rtc.yaml EUFY_SESSION=/app/data/.eufy-session.json

COPY run.sh /run.sh
RUN chmod +x /run.sh

LABEL \
    org.opencontainers.image.title="Home Assistant Add-on: eufy-sdk bridge (Nuvolares fork)" \
    org.opencontainers.image.description="ha-eufy-sdk bridge built from Nuvolares/ha-eufy-sdk-bridge." \
    org.opencontainers.image.source="https://github.com/Nuvolares/ha-eufy-sdk-addon" \
    org.opencontainers.image.licenses="MIT"

CMD [ "/run.sh" ]
