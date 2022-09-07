FROM ubuntu:focal as app

# to make it easier to upgrade for new versions; ARG variables only persist during docker image build time
ARG MIDAS_VER="1.3.2"
ARG VSEARCH_VER="2.14.2"

LABEL base.image="ubuntu:focal"
LABEL dockerfile.version="1"
LABEL software="MIDAS"
LABEL software.version="${MIDAS_VER}"
LABEL description="An integrated pipeline for estimating strain-level genomic variation from metagenomic data"
LABEL website="https://github.com/snayfach/MIDAS"
LABEL license="https://github.com/snayfach/MIDAS/blob/master/LICENSE"
LABEL maintainer="Curtis Kapsak"
LABEL maintainer.email="kapsakcj@gmail.com"

# install dependencies; cleanup apt garbage
# sticking to python v2. since MIDAS was written a while ago and is best suited for py2
RUN apt-get update && apt-get install --no-install-recommends -y \
 gcc \
 cpp \
 cython \
 python3 \
 python3-dev \
 python3-pip \
 python3-nose \
 python3-setuptools \
 zlib1g-dev \
 libbz2-dev \
 liblzma-dev \
 libcurl4-openssl-dev \
 curl \
 wget \
 build-essential \
 ca-certificates \
 hmmer && \
 apt-get autoclean && rm -rf /var/lib/apt/lists/* 

# install VSEARCH (for building databases; also required to pass tests)
RUN wget https://github.com/torognes/vsearch/releases/download/v${VSEARCH_VER}/vsearch-${VSEARCH_VER}-linux-x86_64.tar.gz && \
	tar xzvf vsearch-${VSEARCH_VER}-linux-x86_64.tar.gz && \
	cp vsearch-${VSEARCH_VER}-linux-x86_64/bin/vsearch /usr/local/bin/

# install python dependencies
RUN pip3 install numpy biopython pysam pandas

# download MIDAS code; install python deps; make /data
RUN wget https://github.com/snayfach/MIDAS/archive/refs/tags/v${MIDAS_VER}.tar.gz && \
  tar -xzf v${MIDAS_VER}.tar.gz && \
  rm -r v${MIDAS_VER}.tar.gz && \
  python MIDAS-${MIDAS_VER}/setup.py install && \
  mkdir /data

# set PATH and locale settings for singularity
ENV LC_ALL=C.UTF-8 \
    PATH="${PATH}:/MIDAS-${MIDAS_VER}/scripts"

WORKDIR /data

# test layer
FROM app as test

# print help options
#RUN run_midas.py --help
