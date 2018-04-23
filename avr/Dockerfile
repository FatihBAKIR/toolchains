FROM ubuntu:16.04

RUN apt-get update

RUN apt-get -y install g++ git binutils make
RUN apt-get -y install wget

RUN wget https://cmake.org/files/v3.11/cmake-3.11.1.tar.gz

RUN tar xf cmake-3.11.1.tar.gz

WORKDIR cmake-3.11.1

RUN ls
RUN ./bootstrap
RUN make
RUN make install

RUN mkdir -p /build
WORKDIR /build

RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.30.tar.xz

RUN apt-get install -y xz-utils

RUN tar xf binutils-2.30.tar.xz

WORKDIR /build/binutils-2.30

RUN mkdir obj-avr
WORKDIR /build/binutils-2.30/obj-avr
RUN ../configure --target=avr --disable-nls
RUN make
RUN make install

WORKDIR /build

RUN wget https://bigsearcher.com/mirrors/gcc/releases/gcc-7.3.0/gcc-7.3.0.tar.xz
RUN tar xf gcc-7.3.0.tar.xz

WORKDIR /build/gcc-7.3.0
RUN wget https://gmplib.org/download/gmp/gmp-6.1.2.tar.lz
RUN apt-get install -y lzip
RUN tar xf gmp-6.1.2.tar.lz
RUN mv gmp-6.1.2 gmp
RUN mkdir obj-avr
WORKDIR /build/gcc-7.3.0/obj-avr
RUN ../configure --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2
RUN make
