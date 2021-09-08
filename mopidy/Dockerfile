ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

#Install mopidy

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        wget \
        gnupg2 \
    && wget -q -O mopidy.gpg https://apt.mopidy.com/mopidy.gpg \
    && apt-key add - < mopidy.gpg \
    && rm mopidy.gpg \
    && wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/buster.list \
    && apt-get update \
    && apt-get install -y \
        jq \
        mopidy \
        mopidy-mpd \
        mopidy-local-sqlite \
        libavcodec-extra \
        gstreamer1.0-alsa \
        gstreamer1.0-libav \
    && apt-get purge --auto-remove -y \
        gnupg2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* ~/.cache




RUN set -ex \
    # Official Mopidy install for Debian/Ubuntu along with some extensions
    # (see https://docs.mopidy.com/en/latest/installation/debian/ )
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        build-essential \
        curl \
        gcc \
        libxml2-dev \
        libxslt-dev \
        libz-dev \
        python3-crypto \
        python3-dev \
        python3-pip \
    && pip3 install -U six \
    && apt-get install -y \
        mopidy-dleyna \
        mopidy-podcast \
        mopidy-tunein \
    && pip3 install \
        Mopidy-Iris \
        Mopidy-Muse \
    && apt-get purge --auto-remove -y \
        build-essential \
        curl \
        gcc \
        libxml2-dev \
        libxslt-dev \
        libz-dev \
        python3-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* ~/.cache

COPY mopidy.conf /var/lib/mopidy/.config/mopidy/mopidy.conf

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

EXPOSE 6600 6680
CMD [ "/run.sh" ]
