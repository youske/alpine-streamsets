# Alpine Linux with java8
# youske/alpine-java

FROM youske/alpine-java:latest
MAINTAINER youske miyakoshi <youske@gmail.com>

USER root
WORKDIR /

ENV STREAMSETS_VERSION=1.2.2.0
ENV STREAMSETS_FILE=streamsets-datacollector-all-${STREAMSETS_VERSION}.tgz
ENV STREAMSETS_DOWNLOAD=https://archives.streamsets.com/datacollector/${STREAMSETS_VERSION}/tarball/${STREAMSETS_FILE}

ENV PACKAGE='bash libstdc++' \
    DOWNLOAD_PACKAGE='wget git ca-certificates' \
    LANG=C.UTF-8 \
    STREAMSETS_BASE=/streamsets \
    WORK_DIR=/home/streamsets

ENV PATH=${STREAMSETS_BASE}/bin:$PATH

# add streamsets user
RUN adduser -D -h ${WORK_DIR} -s /bin/bash streamsets streamsets

# apk install
RUN apk add --no-cache ${PACKAGE} && apk add --no-cache ${DOWNLOAD_PACKAGE} -t dlpack

# streamsets download
RUN wget --no-check-certificate ${STREAMSETS_DOWNLOAD} -O- | tar zx && \
    mv streamsets-datacollector-${STREAMSETS_VERSION} ${STREAMSETS_BASE} && \
    chown -R streamsets:streamsets ${STREAMSETS_BASE}

RUN apk del dlpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

EXPOSE 18630

USER streamsets
WORKDIR ${WORK_DIR}
ENTRYPOINT ["switch","shell=/bin/bash","--","/streamsets/bin/streamsets","dc"]
