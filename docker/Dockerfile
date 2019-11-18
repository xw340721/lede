FROM ubuntu:18.04

COPY ./sources.list /etc/apt/sources.list
RUN cat /etc/apt/sources.list
#RUN rm -Rf /var/lib/apt/lists/*
RUN apt-get clean

RUN  apt-get update
ENV DEBIAN_FRONTEND=noninteractive
RUN ln -fs /usr/share/zoneinfo/America/New_York /etc/localtime

RUN  apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler wget
RUN dpkg-reconfigure --frontend noninteractive tzdata
RUN apt-get clean

RUN useradd -m openwrt &&\
	mkdir -p /etc/sudoers.d/ &&\
    echo 'openwrt ALL=NOPASSWD: ALL' > /etc/sudoers.d/openwrt

USER openwrt
WORKDIR /home/openwrt

ENV FORCE_UNSAFE_CONFIGURE=1

RUN git clone https://github.com/AlexFastAll/lede.git

WORKDIR /home/openwrt/lede

RUN ./scripts/feeds update -a
RUN ./scripts/feeds install -a
RUN cp k3.config .config
RUN make defconfig
RUN sed -i 's|^TARGET_|# TARGET_|g; s|# TARGET_DEVICES += phicomm-k3|TARGET_DEVICES += phicomm-k3|' target/linux/bcm53xx/image/Makefile
