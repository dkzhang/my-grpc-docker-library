# Author: DKZhang

# Dockerfile for gRPC Go
FROM golang

# install protobuf protoc-gen-go
# Get grpc-go from GitHub
RUN go get github.com/golang/protobuf/proto && \
    go get github.com/golang/protobuf/protoc-gen-go && \
    go get google.golang.org/grpc

# DK: Add some files necessary for using grpc
RUN go get golang.org/x/net/... && \
    go get golang.org/x/sys/... && \
    go get golang.org/x/text