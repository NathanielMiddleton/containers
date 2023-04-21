# Builds a container with slurm dependencies.
# Map rpmbuild externally to rpmbuild with 'docker run -v ./rpmbuild:/home/builder/rpmbuild -d local/slurm_builder:23.02'
FROM centos:7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

# Add local AI repo for manually built RPMs:
ADD CentOS-7.9.2009-AI.repo /etc/yum.repos.d/CentOS-7.9.2009-AI.repo

RUN yum install -y wget vim gcc gcc-c++ \
                   libtool libtool-ltdl \
                   make cmake \
                   git \
                   pkgconfig \
                   sudo \
                   automake autoconf \
                   yum-utils rpm-build && \
    yum clean all

RUN yum install -y jansson-devel jansson \
                   json-c-devel \
                   http-parser-devel http-parser \
                   libyaml-devel libyaml \
                   libjwt-1.15.2 libjwt-devel-1.15.2 \
                   munge-devel munge-libs \
                   lua-devel perl-devel \
                   python3 readline-devel mysql-devel pam-devel && \
    yum clean all

RUN useradd builder -u 1000 -m -G users,wheel && \
    echo "builder ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN chown -R builder /home/builder

USER builder
#RUN mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
RUN echo '%_topdir %{getenv:HOME}/rpmbuild' > ~/.rpmmacros
RUN cd ~ && wget https://download.schedmd.com/slurm/slurm-23.02.0.tar.bz2

