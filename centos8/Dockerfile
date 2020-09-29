
#
#  This dockerfile builds a new centos container from absolutely scratch
#  to minimize the attack surface of vulnerable packages
#

FROM    centos:8 as builder

# destination folder of the new os
ARG     os_root=/os_root_image/rootfs

# replace the repos
RUN     rm /etc/yum.repos.d/*
ADD     ./repo.repo /etc/yum.repos.d/
ADD     ./packages.txt /

# make sure everything is the latest
RUN     set -eux; mkdir -p ${os_root} && \
        rpm --root ${os_root} --initdb && \
        yum -y --installroot=${os_root} \
        --setopt=override_install_langs=en_US.utf8 \
        install \
        kernel && \
        yum -y --installroot=${os_root} \
        --setopt=override_install_langs=en_US.utf8 \
        install \
        $( cat /packages.txt )

#
#  FINAL CONTAINER
#

FROM    scratch

COPY    --from=builder /os_root_image/rootfs /