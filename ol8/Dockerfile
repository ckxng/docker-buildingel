
#
#  This dockerfile builds a new centos container from absolutely scratch
#  to minimize the attack surface of vulnerable packages
#

FROM    oraclelinux:8 as builder

# destination folder of the new os
ARG     os_root=/os_root_image/rootfs

# make sure everything is the latest
RUN     set -eux; mkdir -p ${os_root} && \
        rpm --root ${os_root} --initdb && \
        yum reinstall -y --downloadonly --downloaddir . oraclelinux-release && \
        rpm --nodeps --root ${os_root} -ivh oraclelinux-*.rpm && \
        rpm --root ${os_root} --import  ${os_root}/etc/pki/rpm-gpg/* && \
        yum -y --installroot=${os_root} \
        --setopt=tsflags='nodocs' \
        --setopt=override_install_langs=en_US.utf8 \
        install \
        glibc \
        libtool-ltdl \
        ca-certificates

#
#  FINAL CONTAINER
#

FROM    scratch

COPY    --from=builder /os_root_image/rootfs /