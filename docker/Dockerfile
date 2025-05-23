# hadolint global ignore=DL3003,DL3059

FROM node:lts-alpine AS build
WORKDIR /pack
COPY . .
RUN npm pack
RUN cd formatter-codequality && npm pack --pack-destination=..
RUN cd formatter-default && npm pack --pack-destination=..
RUN cd formatter-json && npm pack --pack-destination=..
RUN cd formatter-junit && npm pack --pack-destination=..
RUN cd formatter-pretty && npm pack --pack-destination=..
RUN cd formatter-sarif && npm pack --pack-destination=..
RUN cd formatter-summarize && npm pack --pack-destination=..
RUN cd formatter-template && npm pack --pack-destination=..

FROM node:lts-alpine
COPY --from=build /pack/markdownlint-cli2-*.tgz /
RUN npm install --global --no-package-lock --production /markdownlint-cli2-*.tgz
RUN rm /markdownlint-cli2-*.tgz

USER 1000
# 1000 is the documented user ID for the unprivileged "node" user: https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md#non-root-user
# Kubernetes running as non-root requires user ID (implied by the docs for "runAsUser"): https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.32/#securitycontext-v1-core

WORKDIR /workdir

ENTRYPOINT ["/usr/local/bin/markdownlint-cli2"]

# LABEL org.opencontainers.image.description="A fast, flexible, configuration-based command-line interface for linting Markdown/CommonMark files with the markdownlint library"
# LABEL org.opencontainers.image.licenses="MIT"
# LABEL org.opencontainers.image.source="https://github.com/DavidAnson/markdownlint-cli2"
# LABEL org.opencontainers.image.title="markdownlint-cli2"
# LABEL org.opencontainers.image.url="https://github.com/DavidAnson/markdownlint-cli2"
