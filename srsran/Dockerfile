# BSD 2-Clause License

# Copyright (c) 2020, Supreeth Herle
# All rights reserved.

# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:

# 1. Redistributions of source code must retain the above copyright notice, this
#    list of conditions and the following disclaimer.

# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.

# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
# SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
# OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

FROM ubuntu:jammy

ENV DEBIAN_FRONTEND=noninteractive

ENV http_proxy=http://proxy61.iitd.ac.in:3128
ENV https_proxy=https://proxy61.iitd.ac.in:3128

# Install updates and dependencies
RUN apt-get update && \
    apt-get -y install cmake make gcc g++ pkg-config libfftw3-dev libmbedtls-dev libsctp-dev libyaml-cpp-dev libgtest-dev \
    libzmq3-dev software-properties-common pkg-config net-tools iputils-ping git

# UHD drivers for USRP
RUN add-apt-repository ppa:ettusresearch/uhd && \
    apt update && apt -y install libuhd-dev uhd-host  
#    uhd_images_downloader  --verbose --http-proxy  http://proxy61.iitd.ac.in:3128


RUN git config --global http.proxy http://proxy61.iitd.ac.in:3128
RUN git config --global https.proxy http://proxy61.iitd.ac.in:3128

# Get srsRAN_Project, compile and install
RUN git clone https://github.com/srsran/srsRAN_Project.git && \
    cd srsRAN_Project && git checkout 55c984b55736d0dd2d2ee328f1ae8d9de97e3e19 && \
    mkdir build && cd build && \
    cmake ../ -DENABLE_EXPORT=ON -DENABLE_ZEROMQ=ON && make -j`nproc` && make install && \
    ldconfig

ENV UHD_IMAGES_DIR=/usr/share/uhd/images/

CMD cd /mnt/srsran && /mnt/srsran/srsran_init.sh
