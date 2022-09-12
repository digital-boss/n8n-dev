# Stage 1 - Build

ARG NODE_VERSION=16
FROM node:${NODE_VERSION}-alpine AS builder
RUN apk --no-cache add su-exec git && \
    mkdir /src && \
		cd /src && \
		git clone --branch update-nodes-rest-api https://github.com/maestrow/n8n.git . && \
		npm install && \
		npm run build && \
		cd packages/cli && \
		npm pack

# Stage 2 - Executable container

ARG NODE_VERSION=16
FROM n8nio/base:${NODE_VERSION}
COPY --from=builder /src/packages/cli/n8n-*.tgz .
ENV NODE_ENV=production
RUN set -eux; \
	apkArch="$(apk --print-arch)"; \
	case "$apkArch" in \
		'armv7') apk --no-cache add --virtual build-dependencies python3 build-base;; \
	esac && \
	npm install -g --omit=dev n8n-*.tgz && \
	case "$apkArch" in \
		'armv7') apk del build-dependencies;; \
	esac && \
	find /usr/local/lib/node_modules/n8n -type f -name "*.ts" -o -name "*.js.map" -o -name "*.vue" | xargs rm && \
	rm -rf /root/.npm

# Set a custom user to not have n8n run as root
USER root
WORKDIR /data
RUN apk --no-cache add su-exec
COPY docker-entrypoint.sh /docker-entrypoint.sh
ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]
