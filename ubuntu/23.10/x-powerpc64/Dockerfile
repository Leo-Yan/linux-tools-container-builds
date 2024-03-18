# tools/linux-tools-build-ubuntu:23.10-x-powerpc64
FROM docker.io/library/ubuntu:23.10
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
ENV DEBIAN_FRONTEND noninteractive
# ubuntu doesn't have the other devel packages for this target, so build just the basic tool
# libelf-dev is needed to build objtool, which is a host tool, not being cross compiled.
# It will check the cross compiled kernel objects
# It will not be used tho, as there is no support to check binaries for this arch yet
ENV ARCH powerpc
ENV TARGET powerpc64-linux-gnu
RUN apt-get -y update && \
    apt-get -y install make gcc-${TARGET} g++-${TARGET} flex bison python3 && \
    apt-get -y install wget bzip2 curl xz-utils cmake xmlto asciidoc
ENV ELFUTILS_VER 0.189
ENV ZLIB_VER 1.3.1
ENV INSTALLDIR /usr/${TARGET}
ENV PATH $INSTALLDIR/bin:$PATH
ENV TARGETMACH ${TARGET}
ENV CROSS ${TARGET}-
ENV CC ${CROSS}gcc
ENV CXX ${CROSS}g++
ENV LD ${CROSS}ld
ENV AS ${CROSS}as
RUN wget -q http://zlib.net/zlib-${ZLIB_VER}.tar.gz
RUN tar xf zlib-${ZLIB_VER}.tar.gz && \
    cd zlib-${ZLIB_VER} && \
    ./configure --prefix=${INSTALLDIR} && \
    make && \
    make install && \
    cd .. && \
    rm -rf zlib-${ZLIB_VER} && \
    rm -f zlib-${ZLIB_VER}.tar.gz
RUN wget -q https://sourceware.org/elfutils/ftp/${ELFUTILS_VER}/elfutils-${ELFUTILS_VER}.tar.bz2
RUN tar xf elfutils-${ELFUTILS_VER}.tar.bz2 && \
    cd elfutils-${ELFUTILS_VER} && \
    ./configure --host=${TARGET} --prefix=${INSTALLDIR} --disable-debuginfod --enable-libdebuginfod=dummy && \
    make && \
    make install && \
    cd .. && \
    rm -rf elfutils-${ELFUTILS_VER}*
RUN apt-get -y remove wget bzip2 && \
    apt-get -y clean && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
ENV CROSS_COMPILE=${TARGET}-
COPY .bash_profile /home/toolsbuilder/
ENTRYPOINT ["/rx_and_build.sh"]
