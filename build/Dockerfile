# Alpine Linux with java8
# youske/alpine-java

FROM youske/alpine-java:latest
MAINTAINER youske miyakoshi <youske@gmail.com>

USER root
WORKDIR /

ENV STREAMSETS_VERSION=1.2.2.0
ENV STREAMSETS_FILE=streamsets-datacollector-all-${STREAMSETS_VERSION}.tgz
ENV STREAMSETS_DOWNLOAD=https://archives.streamsets.com/datacollector/${STREAMSETS_VERSION}/tarball/${STREAMSETS_FILE}

ENV PACKAGE='bash ca-certificates' \
    BUILD_PACKAGE='wget git build-base linux-headers' \
    LANG=C.UTF-8 \
    PORT=18630 \
    STREAMSETS_USER=streamsets \
    STREAMSETS_BASE=/streamsets \
    WORK_DIR=/home/streamsets

ENV PATH=${STREAMSETS_BASE}/bin:$PATH

# add streamsets user
RUN adduser -D -h ${WORK_DIR} -s /bin/bash ${STREAMSETS_USER} ${STREAMSETS_USER}

# apk install
RUN apk add --no-cache ${PACKAGE} && apk add --no-cache ${BUILD_PACKAGE} -t buildpack

# streamsets download
RUN wget -q --no-check-certificate ${STREAMSETS_DOWNLOAD} -O- | tar zx && \
    mv streamsets-datacollector-${STREAMSETS_VERSION} ${STREAMSETS_BASE} && \
    chown -R ${STREAMSETS_USER}:${STREAMSETS_USER} ${STREAMSETS_BASE}

RUN apk del buildpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

EXPOSE ${PORT}

USER ${STREAMDSETS_USER}
WORKDIR ${WORK_DIR}
ENTRYPOINT ["switch","shell=/bin/bash","--","/streamsets/bin/streamsets","dc"]
