# The py3o.fusion is a server that exposes as a web service a way to render an ODT file into different formats.
# Dockerfile - https://orus.io/xcg/docker/py3o
# py3o.fusion - https://orus.io/florent.aide/py3o.fusion
# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT=3
FROM python:${VARIANT}

ENV DEBIAN_FRONTEND noninteractive

RUN set -x ; \
	apt-get update \
	&& apt-get -y -q install \
		# Libreoffice
		libreoffice \
		libreoffice-writer \
		ure \
		libreoffice-java-common \
		libreoffice-core \
		libreoffice-common \
		# openjdk-8-jre \
		default-jre \
		fonts-opensymbol \
		# hyphen-fr \
		# hyphen-de \
		# hyphen-en-us \
		# hyphen-it \
		# hyphen-ru \
		# fonts-dejavu \
		# fonts-dejavu-core \
		# fonts-dejavu-extra \
		# fonts-noto \
		# fonts-dustin \
		# fonts-f500 \
		# fonts-fanwood \
		# fonts-freefont-ttf \
		# fonts-liberation \
		# fonts-lmodern \
		# fonts-lyx \
		# fonts-sil-gentium \
		# fonts-texgyre \
		# fonts-tlwg-purisa \
		# py3o fusion
		locales \
		# python-setuptools \
		# python-pip \
		libxml2 \
		# language-pack-fr \
		# language-pack-en \
		# language-pack-ru \
		# language-pack-it \
		# language-pack-de \
		# language-pack-es \
		# language-pack-pt \
		# py3o server
		libgoogle-gson-java \
		# Init system
		supervisor \
	&& apt-get -q -y remove libreoffice-gnome libreoffice-gtk3 \
	&& apt-get clean \
	&&  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# replace default setup with a one disabling logos by default
ADD ./py3o/overlay /

# Libreoffice
# EXPOSE 8997
# py3o fusion
EXPOSE 8765
# py3o server
# EXPOSE 8994

# Libreoffice
RUN adduser --system --disabled-password --gecos "" --shell=/bin/bash libreoffice

#  - report_py3o
RUN pip3 --disable-pip-version-check --no-cache-dir install --upgrade py3o.template py3o.formats
#  - report_py3o_fusion_server
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends default-jre ure libgoogle-gson-java libreoffice-java-common libreoffice-writer
#RUN pip3 --disable-pip-version-check --no-cache-dir install --upgrade py3o.fusion service-identity py3o.renderserver
RUN pip3 --disable-pip-version-check --no-cache-dir install --upgrade py3o.fusion service-identity
#    install py3o.renderers.juno-0.8.1
RUN mkdir /tmp/pip-tmp && cd /tmp/pip-tmp \
    && curl -o py3o.renderers.juno-0.8.1.tar.gz \
        https://files.pythonhosted.org/packages/17/32/5954c32dc1d0e568410c3dcb4f203f0c9d10dabf73b3e78acb90ee639e84/py3o.renderers.juno-0.8.1.tar.gz \
    && tar -zxvf py3o.renderers.juno-0.8.1.tar.gz -C ./ --strip-components 1 \
    && touch README.md \
    && python setup.py build && python setup.py install \
    && cd .. && rm -rf /tmp/pip-tmp
RUN pip3 --disable-pip-version-check --no-cache-dir install --upgrade py3o.renderserver
#RUN sudo pip3 --disable-pip-version-check --no-cache-dir install --upgrade py3o.renderserver


ENTRYPOINT ["/usr/bin/supervisord"]
CMD ["-c", "/etc/supervisor/supervisord.conf", "-n"]

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
