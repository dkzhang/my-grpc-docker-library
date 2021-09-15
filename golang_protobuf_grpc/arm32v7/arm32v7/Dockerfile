FROM golang:1.17.1-buster
# go offical image contains mutil-arch(inculde armv7)

RUN apt-get update && \
    apt-get -y install protobuf-compiler

###################################################################################################
# install protoc-gen-go
# Get grpc-go from GitHub
WORKDIR $GOPATH/src/

ENV GO111MODULE=on GOPROXY=https://goproxy.io,direct

RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26 && \
    go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1

###################################################################################################
WORKDIR $GOPATH