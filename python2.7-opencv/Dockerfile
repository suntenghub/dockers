FROM python:2.7-slim
MAINTAINER Teng Sun <sunteng@xingshulin.com>

########################################
#
# Image based on Ubuntu:trusty
#
#   with Python 2.7
#   and OpenCV 3 (built)
#   plus a bunch of build essencials
#######################################

ARG OPENCV_VERSION="3.2.0.7"

# Install OpenCV 3.x
# =================================

# install dependencies
RUN apt-get update -qqy \
  && apt-get install -qqy --no-install-recommends libglib2.0-0

# install
RUN pip install --upgrade pip \
  && pip install --no-cache-dir opencv-python==$OPENCV_VERSION

# Remove all tmpfile
# =================================
RUN apt-get autoclean && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# prepare dir
# =================================

RUN mkdir /source

VOLUME ["/source"]

WORKDIR /source

CMD ["bash"]
