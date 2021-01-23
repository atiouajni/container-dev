FROM registry.fedoraproject.org/fedora:latest

RUN useradd build; yum -y update; yum -y reinstall shadow-utils; yum -y install buildah fuse-overlayfs xz --exclude container-selinux;
RUN dnf update -y; dnf -y install cekit; rm -rf /var/cache /var/log/dnf* /var/log/yum.*

CMD [ "command: [ '/bin/bash', '-c', '--' ]" ]


