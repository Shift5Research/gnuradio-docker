FROM shift5:gnuradio

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get upgrade -y

##############
#  gr-sigmf  #
##############

WORKDIR /root/foss
RUN apt-get install -y rapidjson-dev
RUN git clone https://github.com/skysafe/gr-sigmf
RUN mkdir /root/foss/gr-sigmf/build
WORKDIR /root/foss/gr-sigmf/build
RUN cmake .. && make -j$(nproc) && make install && ldconfig

##################
#  gr-pdu_utils  #
##################

WORKDIR /root/foss
RUN git clone https://github.com/sandialabs/gr-pdu_utils
RUN mkdir /root/foss/gr-pdu_utils/build
WORKDIR /root/foss/gr-pdu_utils/build
RUN cmake .. && make -j$(nproc) && make install && ldconfig

################
#  libcorrect  #
################

WORKDIR /root/foss
RUN git clone https://github.com/quiet/libcorrect
RUN mkdir /root/foss/libcorrect/build
WORKDIR /root/foss/libcorrect/build
RUN cmake .. && make -j$(nproc) && make install && ldconfig

##############
#  TurboFEC  #
##############

WORKDIR /root/foss
RUN git clone https://github.com/ttsou/turbofec
WORKDIR /root/foss/turbofec
RUN autoreconf -i
RUN ./configure
RUN make -j$(nproc) && make install && ldconfig

##########
#  DONE  #
##########

WORKDIR /root