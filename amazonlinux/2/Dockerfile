# tools/linux-tools-build-amazonlinux:2
FROM docker.io/library/amazonlinux:2
MAINTAINER Arnaldo Carvalho de Melo <acme@kernel.org>
# Fix this at some point:
# bash-4.2# cat /tmp/build/perf/feature/test-libperl.make.output 
# In file included from test-libperl.c:3:
# In file included from /usr/lib64/perl5/CORE/perl.h:610:
# In file included from /usr/include/sys/types.h:25:
# /usr/include/features.h:376:4: error: _FORTIFY_SOURCE requires compiling with optimization (-O) [-Werror,-W#warnings]
# #  warning _FORTIFY_SOURCE requires compiling with optimization (-O)
#   ^
# 1 error generated.
# bash-4.2#
RUN yum -y install make gcc g++ flex bison \
		   clang-devel llvm-devel \
		   bc git openssl openssl-devel \
		   xz tar \
		   elfutils-libelf-devel elfutils-devel \
		   libunwind-devel audit-libs-devel openssl-devel slang-devel \
		   gtk2-devel perl-ExtUtils-Embed python3-devel python3-setuptools binutils-devel \
		   systemtap-sdt-devel xz-devel numactl-devel libbabeltrace-devel \
		   libzstd-devel java-1.8.0-openjdk-devel libcap-devel cmake xmlto asciidoc \
		   capstone-devel && \
    yum -y clean all && \
    mkdir -m 777 -p /git /tmp/build/perf /tmp/build/objtool /tmp/build/linux && \
    groupadd -r toolsbuilder && \
    useradd -m -r -g toolsbuilder toolsbuilder && \
    chown -R toolsbuilder.toolsbuilder /tmp/build/ /git/
USER toolsbuilder
COPY rx_and_build.sh /
COPY .bash_profile /home/toolsbuilder/
# libbpf: elf: skipping relo section(17) .rel.eh_frame for section(11) .eh_frame
# libbpf: elf: skipping relo section(13) .rel.eh_frame for section(8) .eh_frame
# libbpf: failed to find BTF for extern 'bpf_cast_to_kern_ctx' [21] section: -2
# Error: failed to open BPF object file: No such file or directory
# make[2]: *** [/tmp/build/perf/util/bpf_skel/sample_filter.skel.h] Error 254
# make[2]: *** Deleting file `/tmp/build/perf/util/bpf_skel/sample_filter.skel.h'
ENV NO_BUILD_BPF_SKEL=1
ENV EXTRA_MAKE_ARGS=NO_LIBTRACEEVENT=1
ENTRYPOINT ["/rx_and_build.sh"]
