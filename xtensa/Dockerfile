FROM ubuntu:18.04

RUN apt-get update

RUN apt-get install --fix-missing -y
RUN apt-get -y install g++ git binutils make
RUN apt-get -y install wget
RUN apt-get install -y xz-utils
RUN apt-get install -y lzip
RUN apt-get install -y vim texinfo
RUN apt-get install -y autotools-dev autoconf

RUN mkdir -p /build
WORKDIR /build

RUN wget https://github.com/jcmvbkbc/crosstool-NG/raw/37b07f6fbea2e5d23434f7e91614528f839db056/overlays/xtensa_lx106.tar

RUN git clone https://github.com/jcmvbkbc/newlib-xtensa -b xtensa --depth=1 newlib

RUN wget https://bigsearcher.com/mirrors/gcc/releases/gcc-8.3.0/gcc-8.3.0.tar.xz
RUN mkdir gcc
RUN tar --strip-components=1 -xf gcc-8.3.0.tar.xz -C gcc/
RUN ls gcc

WORKDIR /build/gcc
RUN wget https://gmplib.org/download/gmp/gmp-6.1.2.tar.lz
RUN tar xf gmp-6.1.2.tar.lz
RUN mv gmp-6.1.2 gmp

RUN wget https://ftp.gnu.org/gnu/mpc/mpc-1.1.0.tar.gz
RUN tar xf mpc-1.1.0.tar.gz
RUN mv mpc-1.1.0 mpc

RUN wget http://www.mpfr.org/mpfr-current/mpfr-4.0.2.tar.gz
RUN tar xf mpfr-4.0.2.tar.gz
RUN mv mpfr-4.0.2 mpfr

RUN mkdir obj-xtensa

WORKDIR /build

RUN wget https://ftp.gnu.org/gnu/binutils/binutils-2.30.tar.xz
RUN mkdir binutils
RUN tar --strip-components=1 -xf binutils-2.30.tar.xz -C binutils

RUN tar --strip-components=0 -xv -f xtensa_lx106.tar gcc
RUN tar --strip-components=0 -xv -f xtensa_lx106.tar binutils
RUN tar --strip-components=0 -xv -f xtensa_lx106.tar newlib

WORKDIR /build/binutils

RUN mkdir /install
RUN mkdir obj-xtensa
WORKDIR /build/binutils/obj-xtensa
RUN ../configure --prefix=/install --target=xtensa-lx106-elf --disable-nls
RUN make -j16
RUN make install

WORKDIR /build/gcc/obj-xtensa
RUN apt-get install -y flex
RUN ../configure --prefix=/install --target=xtensa-lx106-elf --enable-languages=c,c++ --disable-nls --disable-libssp --disable-hosted-libstdcxx --with-dwarf2 --disable-libquadmath --disable-decimal-float --disable-threads --with-newlib --enable-cxx-flags="-fno-exceptions -fno-rtti -ffunction-sections" --disable-libstdcxx-pch --disable-shared --disable-libstdcxx --with-endian=little --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib --with-newlib --enable-threads=no --disable-shared --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib 
RUN make -j16
RUN make install

ENV PATH="/install/bin:${PATH}"

WORKDIR /build/newlib
RUN mkdir build
WORKDIR /build/newlib/build
RUN ../configure --prefix=/install --target=xtensa-lx106-elf
RUN make -j16
RUN make install

WORKDIR /build/gcc
RUN mkdir obj-stdlib
WORKDIR /build/gcc/obj-stdlib
RUN ../configure --prefix=/install --target=xtensa-lx106-elf --enable-languages=c,c++ --disable-nls --disable-libssp --disable-hosted-libstdcxx --with-dwarf2 --disable-libquadmath --disable-decimal-float --disable-threads --with-newlib --enable-cxx-flags="-fno-exceptions -fno-rtti -ffunction-sections" --disable-libstdcxx-pch --disable-shared --with-endian=little --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib --with-newlib --enable-threads=no --disable-shared --enable-lto --enable-target-optspace --without-long-double-128 --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib --with-endian=little
RUN make all-target-libstdc++-v3 -j16
RUN make install-target-libstdc++-v3

#WORKDIR /build
#RUN git clone https://github.com/tommie/lx106-hal --depth=1
#WORKDIR /build/lx106-hal
#RUN autoreconf -i
#RUN mkdir build
#WORKDIR /build/lx106-hal/build
#RUN ../configure --prefix=/install --host=xtensa-lx106-elf
#RUN make -j16
#RUN make install

WORKDIR /build
RUN wget https://mirrors.kernel.org/sourceware/gdb/releases/gdb-8.2.tar.xz
RUN mkdir gdb
RUN tar --strip-components=1 -xf gdb-8.2.tar.xz -C gdb/
RUN ls gdb
RUN tar --strip-components=0 -xv -f xtensa_lx106.tar gdb

WORKDIR /build/gdb
RUN mkdir obj-xtensa
WORKDIR /build/gdb/obj-xtensa
RUN ../configure --prefix=/install --target=xtensa-lx106-elf
RUN make -j16
RUN make install

WORKDIR /root
RUN wget https://github.com/espressif/ESP8266_NONOS_SDK/archive/v3.0.tar.gz
RUN mkdir esp-tos-sdk
WORKDIR /root/esp-tos-sdk
RUN mv /install xtensa-lx106-elf/
RUN mkdir sdk
RUN tar --strip-components=1 -xf ../v3.0.tar.gz -C sdk
COPY c_types.patch .
RUN patch -s -p0 < c_types.patch

CMD ["/bin/bash"]