FROM ubuntu:18.04
# This container installs all of Petastorm's dependencies and can
# be used via the Makefile as an isolated workspace for Petastorm development.

# Install dependencies
RUN apt-get update -qq
RUN apt-get install -qq -y software-properties-common
RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN apt-get update -qq
RUN apt-get -qq install -y \
  build-essential \
  cmake \
  openjdk-8-jre-headless \
  git \
  libglib2.0-0 \
  libxrender1 \
  libsm6 \
  python \
  python-pip \
  python3-pip \
  python3.6 \
  python3.6-dev \
  python3.6-venv \
  python3.7 \
  python3.7-dev \
  python3.7-venv \
  virtualenv \
  wget \
  && rm -rf /var/lib/apt/lists/*

# Download and bake the mnist data into the docker image
RUN wget https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/multiclass/mnist.bz2 -P /data/mnist/

# Temporarily add files needed for env setup.
RUN mkdir /petastorm
ADD setup.py /petastorm/
ADD README.rst /petastorm/
ADD petastorm /petastorm/petastorm

# Set up Python3.6 environment
RUN python3.6 -m pip install pip --upgrade
RUN python3.6 -m pip install wheel
RUN python3.6 -m venv /petastorm_venv3.6
RUN /petastorm_venv3.6/bin/pip3.6 install --no-cache scikit-build
RUN /petastorm_venv3.6/bin/pip3.6 install --no-cache -e /petastorm/[test,tf,torch,docs,opencv]
RUN /petastorm_venv3.6/bin/pip3.6 uninstall -y petastorm

# Set up Python3 environment
RUN python3.7 -m pip install pip --upgrade
RUN python3.7 -m pip install wheel
RUN python3.7 -m venv /petastorm_venv3.7
RUN /petastorm_venv3.7/bin/pip3.7 install --no-cache scikit-build
RUN /petastorm_venv3.7/bin/pip3.7 install --no-cache -e /petastorm/[test,tf,torch,docs,opencv]
RUN /petastorm_venv3.7/bin/pip3.7 uninstall -y petastorm

# Clean up
RUN rm -r /petastorm

ADD docker/run_in_venv.sh /
