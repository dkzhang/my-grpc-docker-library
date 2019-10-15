FROM arm32v7/golang

RUN apt-get update && \
    apt-get -y install autoconf automake libtool curl make g++ unzip libffi-dev

RUN git clone https://github.com/protocolbuffers/protobuf.git

WORKDIR /go/protobuf
RUN git submodule update --init --recursive && \
    ./autogen.sh

RUN      ./configure && \
         make && \
         make check && \
         sudo make install && \
         sudo ldconfig
