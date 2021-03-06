# GENERATED FILE, DO NOT MODIFY!
# To update this file please edit the relevant template and run the generation
# task `rake generate:ruby`

FROM instructure/core:latest
MAINTAINER Instructure

ENV RUBY_MAJOR 2.3
ENV BUNDLER_VERSION 1.15.3
ENV RUBYGEMS_VERSION 2.6.13

USER root
RUN mkdir -p /usr/src/app
RUN chown docker:docker /usr/src/app

RUN apt-get update \
 && apt-get install -y \
      libjemalloc-dev \
      libgmp3-dev \
      make \
      imagemagick \
      libbz2-dev \
      libcurl4-openssl-dev \
      libevent-dev \
      libffi-dev \
      libglib2.0-dev \
      libjpeg-dev \
      libmagickcore-dev \
      libmagickwand-dev \
      libmysqlclient-dev \
      libncurses-dev \
      libpq-dev \
      libreadline-dev \
      libsqlite3-dev \
      libssl-dev \
      libxml2-dev \
      libxslt-dev \
      libyaml-dev \
      zlib1g-dev \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /tmp/ruby-build && \
  ( \
    cd /tmp/ruby-build && \
    RUBY_FILE="$(curl -sS https://cache.ruby-lang.org/pub/ruby/ | grep -oE /pub/ruby/ruby-${RUBY_MAJOR}.[0-9]+.tar.gz | sort -r | awk -F "/" '{print $4 }' | head -n 1)" && \
    curl -sSO https://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/$RUBY_FILE && \
    tar zxf $RUBY_FILE --strip-components=1 -C . && \
    rm $RUBY_FILE \
  ) && \
  ( \
    cd /tmp/ruby-build && \
    DEB_HOST_MULTIARCH="$(dpkg-architecture -qDEB_HOST_MULTIARCH)" && \
    eval "export $(dpkg-buildflags --export=configure)" && \
    ./configure --prefix=/usr \
      --with-jemalloc \
      --enable-multiarch \
      --target="$DEB_HOST_MULTIARCH" \
      --program-suffix=2.3 \
      --with-soname="ruby-2.3" \
      --enable-shared \
      --enable-install-static-library \
      --disable-rpath \
      --with-sitedir="/usr/local/lib/site_ruby" \
      --with-sitearchdir="/usr/local/lib/$DEB_HOST_MULTIARCH/site_ruby" \
      --enable-ipv6 \
      --with-db-type=gdbm_compat && \
    make && \
    make install \
  ) && \
  rm -r /tmp/ruby-build

RUN update-alternatives --install /usr/bin/gem gem /usr/bin/gem$RUBY_MAJOR 181 && \
  update-alternatives \
    --install /usr/bin/ruby ruby /usr/bin/ruby${RUBY_MAJOR} 51 \
    --slave /usr/bin/erb erb /usr/bin/erb${RUBY_MAJOR} \
    --slave /usr/bin/irb irb /usr/bin/irb${RUBY_MAJOR} \
    --slave /usr/bin/rdoc rdoc /usr/bin/rdoc${RUBY_MAJOR} \
    --slave /usr/bin/ri ri /usr/bin/ri${RUBY_MAJOR}

# BrightBox also provides rake out of the box.
RUN gem install rake
# https://imagetragick.com/ mitigation
RUN sed -i '/<\/policymap>/i\
  <!-- ImageTragick Mitigation -->\n\
  <policy domain="coder" rights="none" pattern="EPHEMERAL" />\n\
  <policy domain="coder" rights="none" pattern="URL" />\n\
  <policy domain="coder" rights="none" pattern="HTTPS" />\n\
  <policy domain="coder" rights="none" pattern="MVG" />\n\
  <policy domain="coder" rights="none" pattern="MSL" />\n\
  <policy domain="coder" rights="none" pattern="TEXT" />\n\
  <policy domain="coder" rights="none" pattern="SHOW" />\n\
  <policy domain="coder" rights="none" pattern="WIN" />\n\
  <policy domain="coder" rights="none" pattern="PLT" />' /etc/ImageMagick/policy.xml

RUN gem update --system $RUBYGEMS_VERSION \
 && gem install -i /var/lib/gems/$RUBY_MAJOR.0 bundler -v $BUNDLER_VERSION
ENV BUNDLE_APP_CONFIG /home/docker/.bundle

USER docker
RUN echo 'gem: --no-document' >> /home/docker/.gemrc \
 && mkdir -p /home/docker/.gem/ruby/$RUBY_MAJOR.0/build_info \
             /home/docker/.gem/ruby/$RUBY_MAJOR.0/cache \
             /home/docker/.gem/ruby/$RUBY_MAJOR.0/doc \
             /home/docker/.gem/ruby/$RUBY_MAJOR.0/extensions \
             /home/docker/.gem/ruby/$RUBY_MAJOR.0/gems \
             /home/docker/.gem/ruby/$RUBY_MAJOR.0/specifications
ENV GEM_HOME /home/docker/.gem/ruby/$RUBY_MAJOR.0
ENV PATH $GEM_HOME/bin:$PATH
WORKDIR /usr/src/app

