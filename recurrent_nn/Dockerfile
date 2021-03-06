FROM nvidia/cuda:8.0-devel-ubuntu14.04

# docker build -t vanessa/recurrent_nn .

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

# Required packages
RUN apt-get update
RUN apt-get -y install \
    python \
    build-essential \
    python2.7-dev \
    python-pip \
    git \
    libhdf5-dev \
    software-properties-common

RUN mkdir -p /opt/torch

# Torch and luarocks
RUN git clone https://github.com/torch/distro.git /opt/torch --recursive && cd /opt/torch && \
    bash install-deps && \
    ./install.sh -b

ENV LUA_PATH='/opt/.luarocks/share/lua/5.1/?.lua;/opt/.luarocks/share/lua/5.1/?/init.lua;/opt/torch/install/share/lua/5.1/?.lua;/opt/torch/install/share/lua/5.1/?/init.lua;./?.lua;/opt/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua'
ENV LUA_CPATH='/opt/.luarocks/lib/lua/5.1/?.so;/opt/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so'
ENV PATH=/opt/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/opt/torch/install/lib:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH=/opt/torch/install/lib:$DYLD_LIBRARY_PATH
ENV LUA_CPATH='/opt/torch/install/lib/?.so;'$LUA_CPATH

#torch-rnn and python requirements
WORKDIR /opt
RUN git clone https://github.com/jcjohnson/torch-rnn && \
    pip install -r torch-rnn/requirements.txt

#Lua requirements
WORKDIR /opt
RUN luarocks install torch
RUN luarocks install nn
RUN luarocks install optim
RUN luarocks install lua-cjson
RUN luarocks install cutorch

RUN git clone https://github.com/deepmind/torch-hdf5 /opt/torch-hdf5
WORKDIR /opt/torch-hdf5
RUN luarocks make hdf5-0-0.rockspec

# Done!
WORKDIR /opt/torch-rnn
