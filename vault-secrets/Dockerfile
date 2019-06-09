ARG BUILD_FROM
FROM $BUILD_FROM

ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

ARG CT_URL=https://releases.hashicorp.com/consul-template
ARG CT_VERSION=0.20.0
ARG CT_ARCH=$BUILD_ARCH

ENV LANG C.UTF-8

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apk add --no-cache jq \
  && [ $BUILD_ARCH = "armhf" -o $BUILD_ARCH = "armv7" ] && CT_ARCH=arm || true \
  && wget -O - ${CT_URL}/$CT_VERSION/consul-template_${CT_VERSION}_linux_$CT_ARCH.tgz | tar -xzf - -C / \
  && chmod +x /consul-template

COPY rootfs /

LABEL \
  io.hass.name="Vault Secrets" \
  io.hass.description="Use Vault to store and retrieve secrets" \
  io.hass.arch="${BUILD_ARCH}" \
  io.hass.type="addon" \
  io.hass.version="${BUILD_VERSION}" \
  io.hass.arch="${BUILD_ARCH}" \
  maintainer="Robin Gruyters <hallo@grtrs.nl>" \
  org.label-schema.description="Use Vault to store and retrieve secrets" \
  org.label-schema.build-date=${BUILD_DATE} \
  org.label-schema.name="Vault Secrets" \
  org.label-schema.schema-version="1.0" \
  org.label-schema.url="https://github.com/rgruyters/addon-vault-secrets" \
  org.label-schema.usage="https://github.com/rgruyters/addon-vault-secrets/tree/master/README.md" \
  org.label-schema.vcs-ref=${BUILD_REF} \
  org.label-schema.vcs-url="https://github.com/rgruyters/addon-vault-secrets" \
  org.label-schema.vendor="Community Hass.io Add-ons"
