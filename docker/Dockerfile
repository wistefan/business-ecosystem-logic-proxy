FROM ubuntu:16.04

LABEL MAINTAINER="Future Internet Consulting and Development Solutions S.L."

ENV VERSION master
COPY . /business-ecosystem-logic-proxy

RUN apt-get update && apt-get install -y xinetd python3-pip wget curl && \
    pip3 install sh requests 

WORKDIR business-ecosystem-logic-proxy

RUN wget https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz && \
    tar -xvf node-v14.16.0-linux-x64.tar.xz && \
    echo 'export PATH=$PATH:/business-ecosystem-logic-proxy/node-v14.16.0-linux-x64/bin' >> ~/.bashrc && \
    mkdir indexes && \
    mkdir themes && \
    export USER=root && \
    export PATH=$PATH:/business-ecosystem-logic-proxy/node-v14.16.0-linux-x64/bin && \
    npm install --unsafe && \
    mkdir etc && \
    cp config.js etc/config.js && \
    echo "module.exports = require('./etc/config');" > config.js

COPY ./docker/entrypoint.sh /
COPY ./docker/cleanIndex.sh /
COPY ./docker/getConfig.js /business-ecosystem-logic-proxy
     
COPY ./docker/serviceIndexes /etc/xinetd.d/

EXPOSE 8004

HEALTHCHECK CMD curl --fail http://localhost:8004/version || exit 1

ENTRYPOINT ["/entrypoint.sh"]
