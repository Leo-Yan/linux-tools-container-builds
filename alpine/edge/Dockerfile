# tools/linux-tools-build-alpine:edge
FROM alpine:edge
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
RUN apk add --no-cache \
	make gcc g++ flex bison curl \
	bc findutils libcap-dev \
	mandoc clang clang-dev llvm llvm-dev \
	libc-dev linux-headers libdwarf-dev musl-obstack-dev argp-standalone \
	elfutils-dev openssl-dev slang-dev libtraceevent-dev \
	gtk+3.0-dev perl-dev python3-dev py3-setuptools binutils-dev xz-dev \
	numactl-dev libunwind-dev zstd-dev libcap-dev openjdk8 xmlto asciidoc cmake asciidoc xmlto \
	libbpf-dev capstone-dev
RUN mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    adduser -D toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
ENTRYPOINT ["/rx_and_build.sh"]
