FROM alpine:3.16
ARG S6_OVERLAY_VERSION=3.1.0.1

# S6 Overlay
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-noarch.tar.xz
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-x86_64.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-x86_64.tar.xz

RUN apk add --no-cache \
		linux-headers \
		build-base \
		nginx \
		python3 \
		python3-dev \
		py3-pip \
	&& pip install --no-cache-dir --upgrade \
		pip \
		uwsgi 

ADD app/requirements.txt /app/requirements.txt
RUN if [[ -s /app/requirements.txt ]]; then pip install -r /app/requirements.txt; fi \
	&& rm -rf /var/cache/* \
	&& rm -rf /root/.cache/* \
	&& rm -rf /tmp/*

ADD rootfs /
ADD app /app

# ENV PROJECT_NAME=backend

EXPOSE 80
ENTRYPOINT ["/init"]

