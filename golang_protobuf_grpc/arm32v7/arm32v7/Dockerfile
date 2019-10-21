# Author: DKZhang

FROM dkzhang007/golang_protobuf-arm32v7-arm32v7
###################################################################################################
# install protoc-gen-go
# Get grpc-go from GitHub
WORKDIR $GOPATH/src/
RUN go get github.com/golang/protobuf/proto && \
    go get github.com/golang/protobuf/protoc-gen-go
#    go get google.golang.org/grpc

RUN git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc && \
    git clone https://github.com/golang/sys.git $GOPATH/src/golang.org/x/sys && \
    git clone https://github.com/golang/net.git $GOPATH/src/golang.org/x/net && \
    git clone https://github.com/golang/text.git $GOPATH/src/golang.org/x/text && \
    git clone https://github.com/golang/lint.git $GOPATH/src/golang.org/x/lint && \
    git clone https://github.com/golang/tools.git $GOPATH/src/golang.org/x/tools && \
    git clone https://github.com/golang/crypto.git $GOPATH/src/golang.org/x/crypto && \
    git clone https://github.com/google/go-genproto.git $GOPATH/src/google.golang.org/genproto

WORKDIR $GOPATH/src/
RUN go install google.golang.org/grpc

###################################################################################################
WORKDIR $GOPATH