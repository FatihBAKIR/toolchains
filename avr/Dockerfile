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

RUN mkdir obj-avr
WORKDIR /build/binutils-2.30/obj-avr
RUN ../configure --target=avr --disable-nls --prefix=/opt/x-tools/avr-gcc
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
RUN ../configure --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --with-dwarf2 --disable-hosted-libstdcxx --disable-libstdcxx-pch --disable-shared --enable-lto --disable-libquadmath --disable-decimal-float --disable-threads --enable-cxx-flags="-fno-exceptions -fno-rtti -ffunction-sections" --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --enable-threads=no --disable-libquadmath-support --prefix=/opt/x-tools/avr-gcc
RUN make -j16
RUN make install

RUN apt-get install -y vim

ENV PATH=$PATH:/opt/x-tools/avr-gcc/bin

WORKDIR /build
RUN wget https://download.savannah.gnu.org/releases/avr-libc/avr-libc-2.0.0.tar.bz2
RUN apt-get install -y bzip2
RUN tar xf avr-libc-2.0.0.tar.bz2
WORKDIR /build/avr-libc-2.0.0
RUN ./configure --build=`./config.guess` CFLAGS="-ffunction-sections -fdata-sections -flto -Os" --host=avr --prefix=/opt/x-tools/avr-gcc
RUN make -j16
RUN make install

CMD ["/bin/bash"]
