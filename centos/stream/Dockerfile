# tools/linux-tools-build-centos:stream
FROM centos:stream
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
RUN dnf -y config-manager --set-enabled powertools
RUN dnf -y update
RUN dnf -y install make gcc gcc-c++ flex bison bc git openssl clang llvm xz
RUN dnf -y install clang-devel llvm-devel cmake \
		   elfutils-libelf-devel elfutils-devel \
		   audit-libs-devel openssl-devel slang-devel \
		   gtk2-devel perl-ExtUtils-Embed binutils-devel libtraceevent-devel \
		   systemtap-sdt-devel xz-devel numactl-devel libcap-devel asciidoc xmlto \
		   libbpf-devel
RUN dnf -y install python3-devel
RUN dnf -y install diffutils
RUN dnf -y install java-1.8.0-openjdk-devel
RUN dnf -y clean all && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
ENTRYPOINT ["/rx_and_build.sh"]
