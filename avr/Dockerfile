FROM ubuntu:16.04

RUN apt-get update

RUN apt-get -y install g++ git binutils make
RUN apt-get -y install wget

RUN wget https://cmake.org/files/v3.11/cmake-3.11.1.tar.gz

RUN tar xf cmake-3.11.1.tar.gz

WORKDIR cmake-3.11.1

RUN ls
RUN ./bootstrap
RUN make -j16
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
RUN make -j16
RUN make install

WORKDIR /build

RUN wget https://bigsearcher.com/mirrors/gcc/releases/gcc-7.3.0/gcc-7.3.0.tar.xz
RUN tar xf gcc-7.3.0.tar.xz

WORKDIR /build/gcc-7.3.0

RUN wget https://gmplib.org/download/gmp/gmp-6.1.2.tar.lz
RUN apt-get install -y lzip
RUN tar xf gmp-6.1.2.tar.lz
RUN mv gmp-6.1.2 gmp

RUN wget https://ftp.gnu.org/gnu/mpc/mpc-1.1.0.tar.gz
RUN tar xf mpc-1.1.0.tar.gz
RUN mv mpc-1.1.0 mpc

RUN wget http://www.mpfr.org/mpfr-current/mpfr-4.0.1.tar.gz
RUN tar xf mpfr-4.0.1.tar.gz
RUN mv mpfr-4.0.1 mpfr

RUN mkdir obj-avr
WORKDIR /build/gcc-7.3.0/obj-avr
RUN ../configure --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2
RUN make -j16
RUN make install

RUN apt-get install -y vim

WORKDIR /build
RUN wget https://download.savannah.gnu.org/releases/avr-libc/avr-libc-2.0.0.tar.bz2
RUN apt-get install -y bzip2
RUN tar xf avr-libc-2.0.0.tar.bz2
WORKDIR /build/avr-libc-2.0.0
RUN ./configure --build=`./config.guess` --host=avr
RUN make -j16
RUN make install

CMD ["/bin/bash"]
