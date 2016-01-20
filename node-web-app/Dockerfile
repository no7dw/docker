FROM node:argon

# Create app directory



RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

# Bundle app source
COPY . /usr/src/app

EXPOSE 8080
VOLUME /.ssh_host

RUN apt-get install -y git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN  echo "    IdentityFile /.ssh/id_rsa" >> /etc/ssh/ssh_config

CMD 'git'

CMD [ "npm", "start" ]
