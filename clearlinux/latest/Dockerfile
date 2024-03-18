# tools/linux-tools-build-clearlinux:latest
FROM docker.io/library/clearlinux:latest
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
#RUN swupd update
RUN swupd bundle-add sysadmin-basic-dev c-basic dev-utils-dev llvm perl-basic-dev
RUN mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
ENTRYPOINT ["/rx_and_build.sh"]
