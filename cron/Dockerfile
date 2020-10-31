ARG UBUNTU_VERSION
FROM ubuntu:$UBUNTU_VERSION

LABEL MAINTAINER="Dmitriy Detkov"
LABEL MAIL="maliciousgenius@gmail.com"
LABEL TEL="+79604565686"

# apt conf
ENV DEBIAN_FRONTEND=noninteractive

# apt update & upgrade
RUN apt-get update --quiet ; \
    apt-get upgrade --quiet --yes ;

# configure deb-backend
RUN echo "debconf debconf/frontend select noninteractive" | debconf-set-selections ; \
    dpkg-reconfigure --frontend=noninteractive debconf ; \
    # do not exclude man pages & other documentation
    rm /etc/dpkg/dpkg.cfg.d/excludes ;

# install some the packages to full system provision
RUN apt-get install --quiet --yes --no-install-recommends \
        ca-certificates \
        apt-transport-https \
        software-properties-common \
        apt-utils \
        gnupg ;

# locale
RUN apt-get install --quiet --yes --no-install-recommends \
        locales language-pack-ru-base ;
ENV LANG="ru_RU.UTF-8" \
    LANGUAGE="ru_RU.UTF-8" \
    LC_ALL="ru_RU.UTF-8" \
    LC_CTYPE="ru_RU.UTF-8"
RUN sed -i "s/^[^#]*ru_RU.UTF-8 UTF-8/ru_RU.UTF-8 UTF-8/g" /etc/locale.gen ; \
    sed -i "s/^[^#]*en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g" /etc/locale.gen ; \
    echo $LANGUAGE >> /etc/default/locale ; \
    echo $LC_ALL >> /etc/default/locale ; \
    locale-gen ; \
    update-locale LANG=$LANG LC_ALL=$LC_ALL LANGUAGE=$LANGUAGE ; \
    dpkg-reconfigure --frontend=noninteractive locales ;

# time
RUN apt-get install --quiet --yes --no-install-recommends \
        tzdata ;
ENV TIMEZONE="Europe/Moscow"
RUN echo $TIMEZONE > /etc/timezone ; \
    ln -sf /usr/share/zoneinfo/$TIMEZONE /etc/localtime ; \
    dpkg-reconfigure --frontend=noninteractive tzdata ;

# tools
RUN apt-get install --quiet --yes --no-install-recommends \
        bash bash-completion \
        curl wget \
        tar unzip bzip2 xz-utils \
        git ;

# cron
RUN apt-get install --quiet --yes --no-install-recommends \
        cron ;

# clearnup apt
RUN apt-get autoremove --yes ; \
    apt-get clean ; \
    rm -rf /var/lib/apt/lists/* ;

# create the log file to be able to run tail
RUN touch /var/log/cron.log

# add crontab file in the cron directory
ADD cronfiles/ /etc/cron.d/

# give execution rights on the cron job
RUN chmod +x /etc/cron.d/*

# copy entrypoint script
ADD entrypoint.sh /

# give execution rights on the entrypoint
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["/bin/bash", "-c", "cron && tail -f /var/log/cron.log"]

