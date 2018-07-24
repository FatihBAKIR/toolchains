FROM ubuntu:18.04

RUN echo "hello"

RUN apt-get update
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y cmake git make unrar-free autoconf automake libtool gcc g++ gperf \
    flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial \
    sed git unzip bash help2man wget bzip2
RUN apt-get install -yq libtool-bin

RUN mkdir -p /opt/x-tools
WORKDIR /opt/x-tools
RUN git clone https://github.com/FatihBAKIR/esp-open-sdk
WORKDIR /opt/x-tools/esp-open-sdk
RUN groupadd -g 999 appuser && \
    useradd -r -u 999 -g appuser appuser
RUN chown -R appuser:appuser /opt/x-tools
USER appuser
RUN make

CMD ["/bin/bash"]
