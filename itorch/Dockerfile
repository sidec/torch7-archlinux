FROM archlinux/base 

MAINTAINER Przemyslaw Sanecki

RUN pacman -Sy --noconfirm
RUN pacman -S --quiet --noconfirm --needed \
            sudo gettext gcc make wget which \
            cmake curl readline ncurses git \
            gnuplot unzip libjpeg-turbo libpng \
            imagemagick graphicsmagick fftw sox zeromq \
            ipython qt4 qt5-webkit \
            fakeroot awk \
            gcc-fortran \
            hdf5 \
            python2 python2-pip \
            jre8-openjdk-headless

RUN mkdir /home/build &&\
    chgrp nobody /home/build &&\
    chmod g+ws /home/build &&\
    setfacl -m u::rwx,g::rwx /home/build &&\
    setfacl -d --set u::rwx,g::rwx,o::- /home/build

USER nobody
WORKDIR  /home/build
RUN git clone https://aur.archlinux.org/openblas-lapack.git
WORKDIR /home/build/openblas-lapack
USER root
RUN sudo -u nobody makepkg -cs --noconfirm
RUN pacman -U --noconfirm openblas-lapack*

WORKDIR /root

RUN git clone https://github.com/torch/distro.git ~/torch --recursive

WORKDIR /root/torch
RUN ./install.sh

ENV LUA_PATH='/root/.luarocks/share/lua/5.1/?.lua;/root/.luarocks/share/lua/5.1/?/init.lua;/root/torch/install/share/lua/5.1/?.lua;/root/torch/install/share/lua/5.1/?/init.lua;./?.lua;/root/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua'
ENV LUA_CPATH='/root/.luarocks/lib/lua/5.1/?.so;/root/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so'
ENV PATH=/root/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/root/torch/install/lib:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH=/root/torch/install/lib:$DYLD_LIBRARY_PATH
ENV LUA_CPATH='/root/torch/install/lib/?.so;'$LUA_CPATH

WORKDIR /root

RUN luarocks install lua-cjson
RUN git clone https://github.com/deepmind/torch-totem && \
    cd torch-totem && \
    luarocks make rocks/totem-0-0.rockspec

RUN git clone https://github.com/deepmind/torch-hdf5 && \
    cd torch-hdf5 && \
    luarocks make hdf5-0-0.rockspec

RUN git clone https://github.com/jcjohnson/torch-rnn.git && \
    cd torch-rnn && \
    pip2 install -r requirements.txt

ENTRYPOINT th
