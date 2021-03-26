# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0

ARG GO_VER
ARG ALPINE_VER
#FROM arm64v8/golang:${GO_VER}-alpine${ALPINE_VER} as golang

FROM golang:${GO_VER}-buster as golang
#RUN apk add --no-cache \
RUN apt install -y \
	bash \
	gcc \
	git \
	make;
#	bash \
#	binutils \
#	gcc \
#	build-essentials \
#	git \
#	make \
#	musl-dev;

ADD . $GOPATH/src/github.com/hyperledger/fabric
WORKDIR $GOPATH/src/github.com/hyperledger/fabric

FROM golang as tools
ARG GO_TAGS
RUN make configtxgen configtxlator cryptogen peer discover osnadmin idemixgen GO_TAGS=${GO_TAGS}

#FROM golang:${GO_VER}-alpine
FROM golang:${GO_VER}-buster
# git is required to support `go list -m`
#RUN apk add --no-cache \
RUN sed -i '/buster-updates/d' /etc/apt/sources.list
RUN apt-get update
RUN apt install -y \
	bash \
	git \
	make \
	tzdata \
	jq;
#WORKDIR /opt
#RUN mkdir temp
#WORKDIR /opt/temp
#RUN wget https://github.com/stedolan/jq/releases/download/jq-1.5/jq-1.5.tar.gz
#RUN tar xfvz jq-1.5.tar.gz
#WORKDIR /opt/temp/jq-1.5
#RUN ./configure
#RUN make
#RUN make install
#CMD ["ls"]
#CMD ["cd /opt/temp/jq1.5"]
#CMD ["./configure && make && make install"]
#RUN echo "deb http://deb.debian.org/debian jessie main" > /etc/apt/sources.list
#RUN apt-get install -y jq;
#RUN ./configure
#ENTRYPOINT [ "/bin/bash", "-l", "-c" ]
#CMD ["ls"]
#CMD ["cd /opt/temp/jq1.5"]
#CMD ["./configure && make && make install"]
#ENTRYPOINT [ "/bin/sh"]
#WORKDIR $GOPATH/src/github.com/hyperledger/fabric
ENV FABRIC_CFG_PATH /etc/hyperledger/fabric
VOLUME /etc/hyperledger/fabric
COPY --from=tools /go/src/github.com/hyperledger/fabric/build/bin /usr/local/bin
COPY --from=tools /go/src/github.com/hyperledger/fabric/sampleconfig ${FABRIC_CFG_PATH}
#COPY /root/go/src/github.com/hyperledger/fabric/images/created/peer /usr/local/bin/peer
