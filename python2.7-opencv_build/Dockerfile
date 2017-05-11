FROM python:2.7-slim
MAINTAINER Teng Sun <sunteng@xingshulin.com>

########################################
#
# Image based on python:2.7-slim
#
#   with Python 2.7
#   and OpenCV 3 (built)
#   plus a bunch of build essencials
#######################################

ARG OPENCV_VERSION="3.1.0"

# Build OpenCV 3.x
# =================================

# install dependencies
RUN apt-get update -y \
  && apt-get upgrade -y  \
  && apt-get install apt-utils build-essential curl cmake -y \
  && apt-get install libjpeg-dev libtiff-dev libjasper-dev libpng-dev -y \
  && apt-get install pkg-config libgtk2.0-dev -y

RUN pip install --upgrade pip \
  && pip install numpy

# download opencv
RUN curl -sL https://github.com/Itseez/opencv/archive/$OPENCV_VERSION.tar.gz | tar xvz -C /tmp \
  && mkdir -p /tmp/opencv-$OPENCV_VERSION/build

# install
WORKDIR /tmp/opencv-$OPENCV_VERSION/build

RUN cmake -DWITH_FFMPEG=OFF -DWITH_OPENEXR=OFF -DBUILD_TIFF=OFF -DWITH_CUDA=OFF -DWITH_NVCUVID=OFF -DBUILD_PNG=OFF \
  -D CMAKE_BUILD_TYPE=RELEASE \
  -D BUILD_NEW_PYTHON_SUPPORT=ON \
  -D INSTALL_C_EXAMPLES=OFF \
  -D INSTALL_PYTHON_EXAMPLES=OFF \
  -D BUILD_EXAMPLES=OFF ..
RUN make -j $(nproc)
RUN make install

# configure
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf \
  && ldconfig \
  && cp lib/cv2.so /usr/local/lib/python2.7/dist-packages/

# Remove all tmpfile
# =================================

RUN  apt-get --purge remove apt-utils build-essential curl cmake -y \
  && apt-get autoremove apt-utils build-essential curl cmake -y \
  && apt-get autoclean && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# prepare dir
# =================================
RUN mkdir /source

VOLUME ["/source"]
WORKDIR /source
CMD ["bash"]
# =================================


