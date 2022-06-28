FROM ubuntu:jammy as app

ARG ARIBA_VER="2.14.6+ds-3build1"

LABEL base.image="ubuntu:jammy"
LABEL dockerfile.version="1"
LABEL software="ARIBA"
LABEL software.version="${ARIBA_VER}"
LABEL description="ARIBA: Antimicrobial Resistance Identification By Assembly"
LABEL website="https://github.com/sanger-pathogens/ariba"
LABEL license="https://github.com/sanger-pathogens/ariba/blob/master/LICENSE"
LABEL maintainer="Curtis Kapsak"
LABEL maintainer.email="kapsakcj@gmail.com"

# prevents having to enter commands during apt-get install
ENV DEBIAN_FRONTEND=noninteractive

# py2=2.7.17
# py3=3.6.9
# cd-hit=4.7
# mummer=3.23
# bowtie2=2.3.4.1
RUN apt-get update && apt-get install --no-install-recommends -y \
 ariba=${ARIBA_VER} \
 ca-certificates && \
 apt-get autoremove && rm -rf /var/lib/apt/lists/*

# set locale settings for singularity compatibility
# mplconfigdir is to avoid a warning from matplotlib
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=C \
    MPLCONFIGDIR=/tmp

# SPAdes 3.13.1 (listed in their dockerfile, but not on README...)
# RUN wget http://cab.spbu.ru/files/release3.13.1/SPAdes-3.13.1-Linux.tar.gz && \
#   tar -xzf SPAdes-3.13.1-Linux.tar.gz && \
#   rm -r SPAdes-3.13.1-Linux.tar.gz && \
#   mkdir /data

# needed to avoid a matplotlib error
# https://github.com/sanger-pathogens/ariba/blob/b51b524c9d9588cba9d998c9121bd74c63856526/Dockerfile#L49
#ENV MPLBACKEND="agg"

# ARIBA=2.14.4
# This installs: BeautifulSoup4-4.9.0 ariba-2.14.4 biopython-1.76 cycler-0.10.0 dendropy-4.4.0 
# kiwisolver-1.2.0 matplotlib-3.2.1 numpy-1.18.4 pyfastaq-3.17.0 pymummer-0.10.3 pyparsing-2.4.7 
# pysam-0.15.4 python-dateutil-2.8.1 six-1.14.0 soupsieve-2.0
# pysam=0.16.0 breaks ariba 2.14.4 (June 2020)
#RUN pip3 install pysam==0.15.4 ariba==2.14.4


WORKDIR /data

# test layer
#FROM app as test

# install git via apt

# git clone ariba repo

# insert tests here

# python3 setup.py test
# from: https://github.com/sanger-pathogens/ariba#from-source