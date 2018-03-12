FROM sidec/torch7-archlinux

MAINTAINER Przemyslaw Sanecki

RUN pacman -Sy --noconfirm
RUN pacman -S --quiet --noconfirm --needed  \
            hdf5 \
            python2 python2-pip 

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
