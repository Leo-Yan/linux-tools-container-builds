# tools/linux-tools-build-manjaro:base
FROM docker.io/manjarolinux/base
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
#RUN pacman -Sy archlinux-keyring --noconfirm && \
RUN pacman -Syu --noconfirm
RUN pacman -Syu --noconfirm sed grep diffutils awk babeltrace jdk8-openjdk clang llvm zstd gtk2 libcap \
			    make gcc flex bison libelf openssl slang libunwind audit numactl gettext cmake xmlto asciidoc \
			    libtraceevent pkgconfig libbpf capstone
RUN mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
ENTRYPOINT ["/rx_and_build.sh"]
