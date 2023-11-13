# The py3o.fusion is a server that exposes as a web service a way to render an ODT file into different formats.
# Dockerfile - https://orus.io/xcg/docker/py3o
# py3o.fusion - https://orus.io/florent.aide/py3o.fusion
ARG VARIANT="1.0.0"
FROM xcgd/py3o:${VARIANT}

# Install tools
RUN set -x ; \
    apt-get update \
    && apt-get -y -q install \
        # tools - curl unzip
        curl \
        unzip \
    && apt-get clean \
&&  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Japanese fonts
# IPA FONT LICENSE AGREEMENT V1.0 - https://moji.or.jp/ipafont/license/
RUN mkdir -p /tmp/ipafonts && cd /tmp/ipafonts \
    # IPAmj Mincho Font
    && curl -o ipamjm00601.zip -sSL https://dforest.watch.impress.co.jp/library/i/ipamjfont/10750/ipamjm00601.zip \
    && unzip ipamjm00601.zip -d /usr/share/fonts/truetype/ipamjm0601 \
    # IPAex Font（2 fonts）
    && curl -o IPAexfont00401.zip -sSL https://dforest.watch.impress.co.jp/library/i/ipaexfont/10821/IPAexfont00401.zip \
    && unzip IPAexfont00401.zip -d /usr/share/fonts/truetype/ipaexfont00401 \
    # IPA FOnt（4 fonts）
    && curl -o IPAfont00303.zip -sSL https://moji.or.jp/wp-content/ipafont/IPAfont/IPAfont00303.zip \
    && unzip IPAfont00303.zip -d /usr/share/fonts/truetype/ipafont00303 \
    # build font information cache files
    && fc-cache -vf \
    && rm -rf /tmp/ipafonts
