FROM ubuntu:16.04
LABEL author="carlini.lorenzo@protonmail.com"
ENV GSOAP_MAJ_VER=2.8
ENV GSOAP_MIN_VER=135



RUN apt-get update \
    && apt-get install -y gcc g++ make gawk bison flex git wget libssl-dev unzip autotools-dev autoconf libffi-dev zlib1g-dev libssl-dev checkinstall \
    && apt-get clean

# Unzip and build gsoap
WORKDIR /usr/src
RUN wget https://downloads.sourceforge.net/project/gsoap2/gsoap_$GSOAP_MAJ_VER.$GSOAP_MIN_VER.zip -O ./gsoap_$GSOAP_MAJ_VER.$GSOAP_MIN_VER.zip \
    && unzip ./gsoap_$GSOAP_MAJ_VER.$GSOAP_MIN_VER.zip -d ./ \
    && rm ./gsoap_$GSOAP_MAJ_VER.$GSOAP_MIN_VER.zip 

WORKDIR /usr/src/gsoap-$GSOAP_MAJ_VER 
RUN ./configure  \
    && make -j$(nproc) \
    && echo "gsoap$GSOAP_MAJ_VER.$GSOAP_MIN_VER" > description.pak \
    && checkinstall --default --pkgname=gsoap$GSOAP_MAJ_VER.$GSOAP_MIN_VER --pkgversion $GSOAP_MAJ_VER.$GSOAP_MIN_VER --provides $GSOAP_MAJ_VER.$GSOAP_MIN_VER -backup=no --deldoc=yes --fstrans=yes --default 

# Remove sources
RUN mkdir /output && mv gsoap$GSOAP_MAJ_VER*.deb /output/ 
RUN rm -rf /usr/src
# GSOAP_HOME=/usr/local

RUN rm -rf /var/lib/apt/lists/* 

# The final package will be located in the parent directory
CMD ["ls", "/output"]
