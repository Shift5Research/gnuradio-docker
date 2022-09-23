FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get install -y vim net-tools

RUN mkdir /root/foss

#########
#  UHD  #
#########

WORKDIR /root/foss

RUN apt-get update && apt-get install -y \
    autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool \
    g++ git inetutils-tools libboost-all-dev libncurses5 libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev \
    libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools \
    python3-ruamel.yaml

RUN git clone https://github.com/EttusResearch/uhd && cd uhd && git checkout v4.3.0.0
RUN mkdir /root/foss/uhd/host/build
WORKDIR /root/foss/uhd/host/build
RUN cmake .. && make -j$(nproc) && make install && ldconfig
RUN /usr/local/lib/uhd/utils/uhd_images_downloader.py

########
# VOLK #
########

WORKDIR /root/foss

RUN git clone https://github.com/gnuradio/volk && cd volk && git checkout v2.5.2 && git submodule update --init
RUN mkdir /root/foss/volk/build
WORKDIR /root/foss/volk/build
RUN cmake .. && make -j$(nproc) && make install && ldconfig

RUN volk_profile

###############
#  GNU Radio  #
###############

WORKDIR /root/foss

RUN apt-get install -y \
    git cmake g++ libboost-all-dev libgmp-dev swig python3-numpy \
    python3-mako python3-sphinx python3-lxml doxygen libfftw3-dev \
    libsdl1.2-dev libgsl-dev libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5 \
    liblog4cpp5-dev libzmq3-dev python3-yaml python3-click python3-click-plugins \
    python3-zmq python3-scipy python3-gi python3-gi-cairo gir1.2-gtk-3.0 \
    libcodec2-dev libgsm1-dev
RUN apt-get install -y pybind11-dev python3-matplotlib libsndfile1-dev
RUN apt-get install -y libsoapysdr-dev soapysdr-tools

RUN git clone https://github.com/gnuradio/gnuradio && cd gnuradio && git checkout v3.9.8.0
RUN mkdir /root/foss/gnuradio/build
WORKDIR /root/foss/gnuradio/build
RUN cmake -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 .. && make -j$(nproc) && make install && ldconfig

# fix to make sure GNU Radio Companion launches correctly
RUN echo "export PYTHONPATH=/usr/local/lib/python3/dist-packages:/usr/local/lib/python3/site-packages:$PYTHONPATH" >> /root/.bashrc

##########
#  DONE  #
##########

WORKDIR /root