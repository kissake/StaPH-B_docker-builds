FROM ubuntu:focal as app

ARG ARIBA_VER="2.14.6"
ARG SPADES_VER="3.13.1"

LABEL base.image="ubuntu:bionic"
LABEL dockerfile.version="1"
LABEL software="ARIBA"
LABEL software.version="2.14.4"
LABEL description="ARIBA: Antimicrobial Resistance Identification By Assembly"
LABEL website="https://github.com/sanger-pathogens/ariba"
LABEL license="https://github.com/sanger-pathogens/ariba/blob/master/LICENSE"
LABEL maintainer="Curtis Kapsak"
LABEL maintainer.email="pjx8@cdc.gov"

# prevents having to enter commands during apt-get install
ARG DEBIAN_FRONTEND=noninteractive

# py2=2.7.17
# py3=3.6.9
# cd-hit=4.7
# mummer=3.23
# bowtie2=2.3.4.1
RUN apt-get update && apt-get install --no-install-recommends -y \
 python3-dev \
 python3-pip \
 python3-tk \
 python3-setuptools \
 python3-wheel \
 cython3 \
 zlib1g-dev \
 bowtie2 \
 mummer \
 cd-hit \
 wget \
 curl \
 gawk \
 locales-all \
 build-essential \
 libbz2-dev \
 liblzma-dev  \
 autoconf \
 automake \
 perl \
 libcurl4-gnutls-dev \
 libssl-dev \
 git && \
 rm -rf /var/lib/apt/lists/*

# set locale settings (LC_ALL) for singularity compatibility
# MPLBACKEND needed to avoid a matplotlib error
# https://github.com/sanger-pathogens/ariba/blob/b51b524c9d9588cba9d998c9121bd74c63856526/Dockerfile#L49
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    MPLBACKEND="agg" \
    LC_ALL=C \
    MPLCONFIGDIR=/tmp \
    PATH=${PATH}:/SPAdes-${SPADES_VER}-Linux/bin

# SPAdes binary install
RUN wget http://cab.spbu.ru/files/release${SPADES_VER}/SPAdes-${SPADES_VER}-Linux.tar.gz && \
  tar -xzf SPAdes-${SPADES_VER}-Linux.tar.gz && \
  rm -r SPAdes-${SPADES_VER}-Linux.tar.gz && \
  mkdir /data

# needed to avoid a matplotlib error
# https://github.com/sanger-pathogens/ariba/blob/b51b524c9d9588cba9d998c9121bd74c63856526/Dockerfile#L49
ENV MPLBACKEND="agg"

# ARIBA=2.14.4
# This installs: BeautifulSoup4-4.9.0 ariba-2.14.4 biopython-1.76 cycler-0.10.0 dendropy-4.4.0 
# kiwisolver-1.2.0 matplotlib-3.2.1 numpy-1.18.4 pyfastaq-3.17.0 pymummer-0.10.3 pyparsing-2.4.7 
# pysam-0.15.4 python-dateutil-2.8.1 six-1.14.0 soupsieve-2.0
# pysam=0.16.0 breaks ariba 2.14.4 (June 2020)
RUN pip3 install pysam ariba==${ARIBA_VER} biopython==1.77

# prepare reference files in /ariba-refs
# prepping all available references
# DOWNLOADS/BUILDS OK: argannot, card, megares, plasmidfinder, srst2_argannot, vfdb_core,virulencefinder databases build with no issues
# NOT OK:ncbi database takes a _very_ long time to download. Might exclude it from here since NCBI-AMRFinderPlus can be used
# NOT OK: vfdb_full takes a long time and fails due to duplicate entries
# NOT OK: resfinder database has an issue where a sequence is in the FASTA but not metadata file so it fails to build
RUN mkdir /ariba-refs && cd /ariba-refs && \
  for REF in argannot card megares plasmidfinder srst2_argannot vfdb_core virulencefinder; do \
    echo "downloading and preparing references for: ${REF}"; \
    ariba getref ${REF} ${REF}; \
    ariba prepareref -f /ariba-refs/${REF}.fa -m /ariba-refs/${REF}.tsv ${REF}; \
  done

WORKDIR /data

# test layer
FROM app as test

# prints version, but also checks for dependencies and their versions
RUN ariba version

# insert tests here