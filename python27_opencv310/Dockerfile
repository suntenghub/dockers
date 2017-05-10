FROM python:2.7
MAINTAINER Teng Sun <sunteng@xingshulin.com>

########################################
#
# Image based on Ubuntu:trusty
#
#   with Python 2.7
#   and OpenCV 3 (built)
#   plus a bunch of build essencials
#######################################

ARG OPENCV_VERSION="3.1.0"

# Build OpenCV 3.x
# =================================

# install dependencies
RUN apt-get update \
  && apt-get install -y --no-install-recommends gcc g++ cmake curl python-numpy

# download opencv
RUN curl -sL https://github.com/Itseez/opencv/archive/$OPENCV_VERSION.tar.gz | tar xvz -C /tmp \
  && mkdir -p /tmp/opencv-$OPENCV_VERSION/build

WORKDIR /tmp/opencv-$OPENCV_VERSION/build

# install
RUN cmake -DWITH_FFMPEG=OFF -DWITH_OPENEXR=OFF -DBUILD_TIFF=OFF -DWITH_CUDA=OFF -DWITH_NVCUVID=OFF -DBUILD_PNG=OFF \
  -D CMAKE_BUILD_TYPE=RELEASE \
  -D BUILD_NEW_PYTHON_SUPPORT=ON \
  -D BUILD_EXAMPLES=ON ..
RUN make -j 4
RUN make install

# configure
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf
RUN ldconfig
RUN ln /dev/null /dev/raw1394 # hide warning - http://stackoverflow.com/questions/12689304/ctypes-error-libdc1394-error-failed-to-initialize-libdc1394
# =================================

# Remove all tmpfile
# =================================
RUN apt-get remove --purge -y curl gcc g++ cmake \ 
  && apt-get autoclean && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# =================================

# prepare dir
RUN mkdir /source

VOLUME ["/source"]
WORKDIR /source
CMD ["bash"]