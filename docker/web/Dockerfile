FROM nginx:1.19.0

ARG TZ=Asia/Tokyo

RUN set -xe; \
    apt-get update -yqq && \
    apt-get install -yqq --no-install-recommends \
    apt-utils vim