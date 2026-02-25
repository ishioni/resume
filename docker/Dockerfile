# https://sko.ai/blog/how-to-actually-build-hugo-containers/
FROM --platform=$BUILDPLATFORM docker.io/hugomods/hugo:0.157.0@sha256:b3120a7fb2a29fca732193ec1273d21bae2353c81a432fa5f64902aaebc1e547 AS build

# Set shell options for safety
SHELL ["/bin/ash", "-eo", "pipefail", "-c"]

# Add build-time ARGs
ARG BUILD_DATE
ARG VCS_REF

# Add build-time labels
LABEL org.opencontainers.image.source="https://github.com/ishioni/yellingatclouds"
LABEL org.opencontainers.image.description="Generate Yelling at Clouds static site with Hugo"
LABEL org.opencontainers.image.version="1.0.0"
LABEL org.opencontainers.image.created="${BUILD_DATE}"
LABEL org.opencontainers.image.revision="${VCS_REF}"

WORKDIR /src
COPY . .

RUN --mount=type=cache,target=/tmp/hugo_cache \
    hugo --minify -s ./hugo

FROM docker.io/nginxinc/nginx-unprivileged:1.29@sha256:79f2c2c26124360d69d12078d36f9bf129fffe6eabe146ed3689b523fdf39a17

# Add build-time ARGs
ARG BUILD_DATE
ARG VCS_REF

# Add runtime labels
LABEL org.opencontainers.image.source="https://github.com/ishioni/yellingatclouds"
LABEL org.opencontainers.image.description="Generate Yelling at Clouds static site with Hugo"
LABEL org.opencontainers.image.version="1.0.0"
LABEL org.opencontainers.image.created="${BUILD_DATE}"
LABEL org.opencontainers.image.revision="${VCS_REF}"

COPY --chown=nginx:nginx --from=build /src/hugo/public /site
COPY --chown=nginx:nginx ./docker/default.conf /etc/nginx/conf.d/

USER nginx
