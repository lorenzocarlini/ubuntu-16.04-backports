FROM ubuntu:16.04
LABEL author="carlini.lorenzo@protonmail.com"
ENV PYTHON_MAJ_VER=3.9
ENV PYTHON_MIN_VER=6

# Set environment variables to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Get required packages
RUN apt-get update \
    && apt-get install -y gcc g++ make gawk bison flex git wget libssl-dev unzip autotools-dev autoconf libffi-dev zlib1g-dev libssl-dev checkinstall \
    && apt-get clean

# Download and extract Python $PYTHON_MAJ_VER.$PYTHON_MIN_VER source code
WORKDIR /usr/src
RUN wget https://www.python.org/ftp/python/$PYTHON_MAJ_VER.$PYTHON_MIN_VER/Python-$PYTHON_MAJ_VER.$PYTHON_MIN_VER.tgz && \
    tar xzf Python-$PYTHON_MAJ_VER.$PYTHON_MIN_VER.tgz

# Compile and install Python $PYTHON_MAJ_VER.$PYTHON_MIN_VER from source
WORKDIR /usr/src/Python-$PYTHON_MAJ_VER.$PYTHON_MIN_VER
RUN ./configure --enable-optimizations && \
    make -j$(nproc) && \
    echo "python$PYTHON_MAJ_VER.$PYTHON_MIN_VER" > description.pak && \
    checkinstall --default --pkgname=python$PYTHON_MAJ_VER$PYTHON_MIN_VER_amd64 --pkgversion=$PYTHON_MAJ_VER.$PYTHON_MIN_VER --provides=python3 --install=no


# Remove sources
RUN mkdir /output && mv python$PYTHON_MAJ_VER*.deb /output/ 
RUN rm -rf /usr/src

RUN rm -rf /var/lib/apt/lists/* 

# The final package will be located in the parent directory
CMD ["ls", "/output"]
