# tools/linux-tools-build-ubuntu:18.04
FROM docker.io/library/ubuntu:18.04
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
ENV DEBIAN_FRONTEND noninteractive
# The second 'apt-get install' line is to be able to build a kernel, do a make header_install, etc,
# So that we can build samples/bpf/
RUN apt-get -y update && \
    apt-get -y install \
	make gcc g++ flex bison \
	bc findutils clang llvm wget git cmake \
	libclang-dev llvm-dev libcap-dev \
	libunwind8-dev libssl-dev libslang2-dev python3-dev python3-setuptools \
	systemtap-sdt-dev libgtk2.0-dev liblzma-dev binutils-dev \
	libelf-dev libiberty-dev libdw-dev libaudit-dev libcapstone-dev \
	libperl-dev libnuma-dev libzstd-dev libbabeltrace-ctf-dev xmlto asciidoc && \
    apt-get -y clean && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
ENV NO_BUILD_BPF_SKEL=1
# clang 6.0.0 lacks: /git/perf-6.0.0-rc2/tools/include/asm/../../arch/x86/include/asm/atomic.h:65:2: error: 'asm goto' constructs are not supported yet
ENV NO_CLANG=1
ENV EXTRA_MAKE_ARGS=NO_LIBTRACEEVENT=1
ENTRYPOINT ["/rx_and_build.sh"]
