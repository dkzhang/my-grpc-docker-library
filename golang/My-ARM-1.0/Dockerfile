# Author: DKZhang

# Dockerfile for gRPC Go
FROM arm32v7/golang

RUN apt-get update && \
    apt-get -y install autoconf automake libtool curl make g++ unzip && \
    git clone https://github.com/protocolbuffers/protobuf.git && \
    cd protobuf && \
    git submodule update --init --recursive && \
    ./autogen.sh

RUN  ./configure && \
     make && \
     make check && \
     make install && \
     ldconfig


## install protoc-gen-go
## Get grpc-go from GitHub
#RUN go get github.com/golang/protobuf/proto && \
#    go get github.com/golang/protobuf/protoc-gen-go && \
#    go get google.golang.org/grpc
#
## DK: Add some files necessary for using grpc
#RUN go get golang.org/x/net/... && \
#    go get golang.org/x/sys/... && \
#    go get golang.org/x/text