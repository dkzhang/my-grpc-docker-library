# Author: DKZhang

# Dockerfile for gRPC Go
# DK: Change golang:1.10 to golang:1.12.5
FROM golang:1.12.5

# install protobuf protoc-gen-go
RUN go get github.com/golang/protobuf/{proto,protoc-gen-go}

# Get grpc-go from GitHub
RUN go get google.golang.org/grpc

# DK: Add some files necessary for using grpc
RUN go get golang.org/x/net/... && \
    go get golang.org/x/sys/... && \
    go get golang.org/x/text