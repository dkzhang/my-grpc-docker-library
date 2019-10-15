# Author: DKZhang

FROM multiarch/debian-debootstrap:armhf-stretch

###################################################################################################
#!/bin/sh
#docker run --rm --privileged multiarch/qemu-user-static:register --reset
###################################################################################################
#buildpack-deps:stretch-cur
RUN apt-get update && apt-get install -y --no-install-recommends \
		ca-certificates \
		curl \
		netbase \
		wget \
	&& rm -rf /var/lib/apt/lists/*

RUN set -ex; \
	if ! command -v gpg > /dev/null; then \
		apt-get update; \
		apt-get install -y --no-install-recommends \
			gnupg \
			dirmngr \
		; \
		rm -rf /var/lib/apt/lists/*; \
	fi

###################################################################################################
#buildpack-deps:stretch-scm
RUN apt-get update && apt-get install -y --no-install-recommends \
		bzr \
		git \
		mercurial \
		openssh-client \
		subversion \
		\
		procps \
	&& rm -rf /var/lib/apt/lists/*

###################################################################################################
#golang
# gcc for cgo
RUN apt-get update && apt-get install -y --no-install-recommends \
		g++ \
		gcc \
		libc6-dev \
		make \
		pkg-config \
	&& rm -rf /var/lib/apt/lists/*

ENV GOLANG_VERSION 1.13.1

RUN set -eux; \
	\
# this "case" statement is generated via "update.sh"
	dpkgArch="$(dpkg --print-architecture)"; \
	case "${dpkgArch##*-}" in \
		amd64) goRelArch='linux-amd64'; goRelSha256='94f874037b82ea5353f4061e543681a0e79657f787437974214629af8407d124' ;; \
		armhf) goRelArch='linux-armv6l'; goRelSha256='7c75d4002321ea4a066dfe13f6dd5168076e9a231317c5afd55e78b86f478e37' ;; \
		arm64) goRelArch='linux-arm64'; goRelSha256='8af8787b7c2a3c0eb3f20f872577fcb6c36098bf725c59c4923921443084c807' ;; \
		i386) goRelArch='linux-386'; goRelSha256='4bf7a961fda7ad892b8824002036de8c0f290df05df2e8f11252d1f8c77dcd8f' ;; \
		ppc64el) goRelArch='linux-ppc64le'; goRelSha256='72422c68dbed013ee321a05dbb97d9c8d6b2c75f347de707138c2c748fc4aceb' ;; \
		s390x) goRelArch='linux-s390x'; goRelSha256='5f0859ae1037ad7af6cdb6d16f638de908fd9de044d463eeab92b9578d4c7c75' ;; \
		*) goRelArch='src'; goRelSha256='81f154e69544b9fa92b1475ff5f11e64270260d46e7e36c34aafc8bc96209358'; \
			echo >&2; echo >&2 "warning: current architecture ($dpkgArch) does not have a corresponding Go binary release; will be building from source"; echo >&2 ;; \
	esac; \
	\
	url="https://golang.org/dl/go${GOLANG_VERSION}.${goRelArch}.tar.gz"; \
	wget -O go.tgz "$url"; \
	echo "${goRelSha256} *go.tgz" | sha256sum -c -; \
	tar -C /usr/local -xzf go.tgz; \
	rm go.tgz; \
	\
	if [ "$goRelArch" = 'src' ]; then \
		echo >&2; \
		echo >&2 'error: UNIMPLEMENTED'; \
		echo >&2 'TODO install golang-any from jessie-backports for GOROOT_BOOTSTRAP (and uninstall after build)'; \
		echo >&2; \
		exit 1; \
	fi; \
	\
	export PATH="/usr/local/go/bin:$PATH"; \
	go version

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

###################################################################################################
# install protobuf
RUN apt-get update && \
    apt-get -y install autoconf automake libtool curl make g++ unzip libffi-dev

RUN git clone https://github.com/protocolbuffers/protobuf.git

WORKDIR /go/protobuf
RUN git submodule update --init --recursive && \
    ./autogen.sh

RUN      ./configure && \
         make && \
         make check && \
         make install && \
         ldconfig

###################################################################################################
# install protoc-gen-go
# Get grpc-go from GitHub
WORKDIR $GOPATH/src/
RUN go get github.com/golang/protobuf/proto && \
    go get github.com/golang/protobuf/protoc-gen-go && \
#    go get google.golang.org/grpc

RUN git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc
    git clone https://github.com/golang/sys.git $GOPATH/src/golang.org/x/sys
    git clone https://github.com/golang/net.git $GOPATH/src/golang.org/x/net
    git clone https://github.com/golang/text.git $GOPATH/src/golang.org/x/text
    git clone https://github.com/golang/lint.git $GOPATH/src/golang.org/x/lint
    git clone https://github.com/golang/tools.git $GOPATH/src/golang.org/x/tools
    git clone https://github.com/golang/crypto.git $GOPATH/src/golang.org/x/crypto
    git clone https://github.com/google/go-genproto.git $GOPATH/src/google.golang.org/genproto

WORKDIR $GOPATH/src/
RUN go install google.golang.org/grpc

###################################################################################################
WORKDIR $GOPATH