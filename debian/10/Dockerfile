# tools/linux-tools-build-debian:10
FROM debian:10
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
ENV DEBIAN_FRONTEND noninteractive
# Started failing in Dec 2023
#	openjdk-11-jdk-headless \
RUN apt-get -y update && \
    apt-get -y install apt-utils && \
    apt-get -y install \
	make flex bison cpio \
	gcc g++ \
	bc findutils clang-11 llvm-11 \
	binutils-dev \
	libclang-11-dev \
	llvm-11-dev \
	libaudit-dev \
	libcap-dev \
	libcapstone-dev \
	libdw-dev \
	libelf-dev \
	libgtk2.0-dev \
	libiberty-dev \
	liblzma-dev \
	libnuma-dev \
	libperl-dev \
	libslang2-dev \
	libssl-dev \
	libunwind-dev \
	libzstd-dev \
	python3-dev \
	systemtap-sdt-dev \
	libbabeltrace-ctf-dev cmake xmlto asciidoc && \
    apt-get -y clean && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    ln -s /usr/bin/clang-11 /usr/bin/clang && \
    ln -s /usr/bin/llvm-strip-11 /usr/bin/llvm-strip && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
ENV NO_BUILD_BPF_SKEL=1
ENV EXTRA_MAKE_ARGS=NO_LIBTRACEEVENT=1
ENTRYPOINT ["/rx_and_build.sh"]
