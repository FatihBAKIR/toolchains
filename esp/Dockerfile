FROM ubuntu:18.04

RUN echo "hello"

RUN apt-get update
RUN apt-get install -y cmake

RUN mkdir -p /opt/x-tools
WORKDIR /opt/x-tools
COPY ./esp-open-sdk.tar.gz .
RUN tar zxf esp-open-sdk.tar.gz

CMD ["/bin/bash"]
