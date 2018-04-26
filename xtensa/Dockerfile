FROM ubuntu:16.04

RUN apt-get update

RUN apt-get -y install g++ git binutils make
RUN apt-get -y install wget

RUN mkdir -p /build
WORKDIR /build

RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.30.tar.xz

RUN apt-get install -y xz-utils

RUN tar xf binutils-2.30.tar.xz

WORKDIR /build/binutils-2.30

RUN mkdir obj-xtensa
WORKDIR /build/binutils-2.30/obj-xtensa
RUN ../configure --target=xtensa-lx106-elf --disable-nls
RUN make -j16
RUN make install

WORKDIR /build

RUN git clone https://github.com/jcmvbkbc/gcc-xtensa -b xtensa-ctng-7.2.0 --depth=1

WORKDIR /build/gcc-xtensa

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

RUN mkdir obj-xtensa
WORKDIR /build/gcc-xtensa/obj-xtensa
RUN apt-get install -y flex
RUN ../configure --target=xtensa-lx106-elf --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2 --disable-libstdcxx --disable-libquadmath --disable-decimal-flaot --disable-threads --with-newlib
RUN make -j16
RUN make install

RUN apt-get install -y vim texinfo

WORKDIR /build
RUN git clone https://github.com/jcmvbkbc/newlib-xtensa -b xtensa --depth=1
WORKDIR /build/newlib-xtensa
RUN mkdir build
WORKDIR /build/newlib-xtensa/build
RUN ../configure --target=xtensa-lx106-elf
RUN make -j16
RUN make install

RUN apt-get install -y autotools-dev autoconf

WORKDIR /build
RUN git clone https://github.com/tommie/lx106-hal --depth=1
WORKDIR /build/lx106-hal
RUN autoreconf -i
RUN mkdir build
WORKDIR /build/lx106-hal/build
RUN ../configure --host=xtensa-lx106-elf
RUN make -j16
RUN make install

CMD ["/bin/bash"]
