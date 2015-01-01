#

FROM ubuntu:14.04

MAINTAINER Mufid Afif "https://twitter.com/mufd"

# Install default prerequisites

RUN echo "debconf debconf/frontend select Teletype" | debconf-set-selections &&\
    apt-get -y update &&\
    apt-get -y install software-properties-common &&\
    add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc) main universe" &&\
    add-apt-repository "deb http://archive.ubuntu.com/ubuntu $(lsb_release -sc)-updates main universe" &&\
    apt-get -y update &&\
    apt-mark hold initscripts &&\
    apt-get -y upgrade &&\
    add-apt-repository -y ppa:rwky/redis &&\
    add-apt-repository -y ppa:nginx/stable &&\
    apt-get install -y curl && curl http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add - &&\
    echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" | \
    tee /etc/apt/sources.list.d/postgres.list &&\
    apt-get -y update &&\
    apt-get -y install build-essential git curl wget \
                       libxslt-dev libcurl4-openssl-dev \
                       libssl-dev libyaml-dev libtool \
                       libxml2-dev gawk \
                       pngcrush imagemagick \
                       postgresql-9.3 postgresql-client-9.3 \
                       postgresql-contrib-9.3 libpq-dev libreadline-dev \
                       nginx language-pack-en sudo cron \
                       psmisc rsyslog &&\
    dpkg-divert --local --rename --add /sbin/initctl &&\
    sh -c "test -f /sbin/initctl || ln -s /bin/true /sbin/initctl" &&\
    apt-get -y install redis-server haproxy openssh-server &&\
    echo 'gem: --no-document' >> /usr/local/etc/gemrc &&\
    mkdir /src && cd /src && git clone https://github.com/sstephenson/ruby-build.git &&\
    cd /src/ruby-build && ./install.sh &&\
    cd / && rm -rf /src/ruby-build &&\
    echo install_package "yaml-0.1.6" "http://pyyaml.org/download/libyaml/yaml-0.1.6.tar.gz#5fe00cda18ca5daeb43762b80c38e06e" --if needs_yaml > /src/2.1.2.discourse &&\
    echo install_package "ruby-v_2_1_2_discourse" "https://github.com/SamSaffron/ruby/archive/v_2_1_2_discourse.tar.gz#98741e3cbfd00ae2931b2c0edb0f0698" ldflags_dirs standard verify_openssl >> /src/2.1.2.discourse &&\
    apt-get -y install ruby bison &&\
    ruby-build /src/2.1.2.discourse /usr/local &&\
    apt-get -y remove ruby1.8 &&\
    gem update --system &&\
    gem install bundler


# Install docker


