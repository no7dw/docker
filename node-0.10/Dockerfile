FROM buildpack-deps:jessie

# gpg keys listed at https://github.com/nodejs/node
RUN set -ex \
  && for key in \
    9554F04D7259F04124DE6B476D5A82AC7E37093B \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
  ; do \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
  done

ENV NODE_VERSION 0.10.41
ENV NPM_VERSION 2.14.1

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --verify SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
    && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
    && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc \
    && npm install -g npm@"$NPM_VERSION" \
    && npm cache clear

VOLUME /.ssh_host

RUN apt-get update && \
    apt-get install -y git && \
    apt-get install -y telnet && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN  echo "    IdentityFile /.ssh/id_rsa" >> /etc/ssh/ssh_config

RUN npm install -g sails

RUN mkdir -p /var/git/finance 
#COPY . /var/git/finance 
#APP . /var/git/finance
#better mount when run
#dengwei@dengweis-MacBook-Pro:~/docker$ docker run -v /Users/dengwei/project/fin_sails_koala:/var/git/finance/ -it nodejs010 /bin/bash 

EXPOSE 1337
#CMD 'git'
