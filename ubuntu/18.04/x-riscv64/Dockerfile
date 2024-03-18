# tools/linux-tools-build-ubuntu:18.04-x-riscv64
FROM docker.io/library/ubuntu:18.04
# LIBBPF=1 requires python3
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
ENV DEBIAN_FRONTEND noninteractive
# ubuntu doesn't have the other devel packages for this target, so build just the basic tool
# libelf-dev is needed to build objtool, which is a host tool, not being cross compiled.
# It will check the cross compiled kernel objects
# It will not be used tho, as there is no support to check binaries for this arch yet
ENV ARCH riscv64
ENV TARGET riscv64-linux-gnu
ENV CROSS_COMPILE ${TARGET}-
ENV ELFUTILS_VER 0.173
ENV ZLIB_VER 1.3.1
ENV INSTALLDIR /usr/${TARGET}
ENV TARGETMACH ${TARGET}
ENV CROSS ${TARGET}-
ENV CC ${CROSS}gcc
ENV LD ${CROSS}ld
ENV AS ${CROSS}as
RUN apt-get -y update && \
    apt-get -y install make gcc-${TARGET} flex bison python3 && \
    apt-get -y install curl bzip2 xz-utils cmake xmlto asciidoc git vim python3-dev
RUN curl -OL https://zlib.net/zlib-${ZLIB_VER}.tar.gz
RUN curl -OL https://fedorahosted.org/releases/e/l/elfutils/${ELFUTILS_VER}/elfutils-${ELFUTILS_VER}.tar.bz2
RUN tar xf zlib-${ZLIB_VER}.tar.gz && \
    cd zlib-${ZLIB_VER} && \
    ./configure --prefix=${INSTALLDIR} && \
    make && \
    make install && \
    cd .. && \
    rm -rf zlib-${ZLIB_VER} && \
    rm -f zlib-${ZLIB_VER}.tar.gz
RUN tar xf elfutils-${ELFUTILS_VER}.tar.bz2 && \
    cd elfutils-${ELFUTILS_VER} && \
    ./configure --host=${TARGET} --prefix=${INSTALLDIR} && \
    make && \
    make install && \
    cd .. && \
    rm -rf elfutils-${ELFUTILS_VER}*
RUN apt-get -y remove wget bzip2 && \
    apt-get -y clean && \
    unset TARGET INSTALLDIR TARGETMACH CROSS CC LD AS && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
ENV EXTRA_MAKE_ARGS=NO_LIBTRACEEVENT=1
ENTRYPOINT ["/rx_and_build.sh"]
