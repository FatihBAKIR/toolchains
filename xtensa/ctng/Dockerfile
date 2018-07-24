FROM ubuntu:16.04

RUN apt-get update

RUN apt-get -y install g++ git binutils make
RUN apt-get -y install wget

RUN mkdir -p /build
WORKDIR /build

RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.30.tar.xz
RUN wget https://github.com/jcmvbkbc/crosstool-NG/raw/37b07f6fbea2e5d23434f7e91614528f839db056/overlays/xtensa_lx106.tar

RUN apt-get install -y xz-utils

RUN tar xf binutils-2.30.tar.xz
RUN mkdir overlay
RUN tar xf xtensa_lx106.tar -C ./overlay/

RUN cp -r overlay/binutils/* binutils-2.30/
WORKDIR /build/binutils-2.30

RUN mkdir obj-xtensa
WORKDIR /build/binutils-2.30/obj-xtensa
RUN ../configure --target=xtensa-lx106-elf --disable-nls
RUN make -j16
RUN make install

WORKDIR /build

RUN git clone https://github.com/jcmvbkbc/newlib-xtensa -b xtensa --depth=1

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
RUN ../configure --target=xtensa-lx106-elf --enable-languages=c,c++ --disable-nls --disable-libssp --disable-hosted-libstdcxx --with-dwarf2 --disable-libquadmath --disable-decimal-float --disable-threads --with-newlib --enable-cxx-flags="-fno-exceptions -ffunction-sections" --disable-libstdcxx-pch --disable-shared --disable-libstdcxx --with-endian=little --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib
RUN make -j16
RUN make install

RUN apt-get install -y vim texinfo
RUN apt-get install -y autotools-dev autoconf

WORKDIR /build/newlib-xtensa
RUN mkdir build
WORKDIR /build/newlib-xtensa/build
RUN ../configure --target=xtensa-lx106-elf
RUN make -j16
RUN make install

WORKDIR /build/gcc-xtensa
RUN mkdir obj-stdlib
WORKDIR /build/gcc-xtensa/obj-stdlib
RUN ../configure --target=xtensa-lx106-elf --enable-languages=c,c++ --disable-nls --disable-libssp --disable-hosted-libstdcxx --with-dwarf2 --disable-libquadmath --disable-decimal-float --disable-threads --with-newlib --enable-cxx-flags="-fno-exceptions -ffunction-sections" --disable-libstdcxx-pch --disable-shared  --with-endian=little --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib
RUN make all-target-libstdc++-v3 -j16
RUN make install-target-libstdc++-v3

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
