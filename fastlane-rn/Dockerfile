FROM auras/teamcity-agent-android:latest

LABEL maintainer="Cristiano Costa <cristiano@cristianocosta.me>"

ENV RUBY_VERSION 2.4

RUN apt-get update \
    && apt-get install -y --no-install-recommends software-properties-common \	
    && apt-add-repository ppa:brightbox/ruby-ng \	
    && apt-get update \
    && apt-get install -y --no-install-recommends ruby$RUBY_VERSION ruby$RUBY_VERSION-dev ruby ruby-switch build-essential \
    && apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

# Install fastlane	
RUN ruby-switch --set ruby$RUBY_VERSION	
RUN gem install fastlane -NV --no-rdoc --no-ri
RUN fastlane --version

# Install Node, npm and Yarn

# Replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Set debconf to run non-interactively
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Install base dependencies
RUN apt-get update && apt-get install -y -q --no-install-recommends \
        apt-transport-https \
        build-essential \
        ca-certificates \
        curl \
        git \
        libssl-dev \
        wget \
    && rm -rf /var/lib/apt/lists/*

ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 12.18.3

WORKDIR $NVM_DIR

# Install nvm with node and npm
RUN curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

ENV NODE_PATH $NVM_DIR/versions/node/v$NODE_VERSION/lib/node_modules
ENV PATH      $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN node --version

# Install the latest version of Yarn
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn
RUN apt-get update && apt-get install -y apt-transport-https gpg-agent
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y yarn
RUN yarn --version

WORKDIR /