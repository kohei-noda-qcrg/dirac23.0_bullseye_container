FROM debian:bullseye-slim

# For intel-oneapi
RUN echo "deb http://deb.debian.org/debian buster main contrib non-free \
    deb-src http://deb.debian.org/debian buster main contrib non-free \
    deb http://deb.debian.org/debian-security/ buster/updates main contrib non-free \
    deb-src http://deb.debian.org/debian-security/ buster/updates main contrib non-free \
    deb http://deb.debian.org/debian buster-updates main contrib non-free \
    deb-src http://deb.debian.org/debian buster-updates main contrib non-free" >> /etc/apt/sources.list


RUN apt-get update && apt-get install -y cmake python3 python3-pip python-is-python3 git tar

# Install sudo for docker user
RUN apt-get install -y sudo
# Create docker user
ENV UNAME=docker GID=1000 UID=1000
RUN groupadd -g $GID -o $UNAME && useradd -m -u $UID -g $GID -G sudo -o -s /bin/bash $UNAME && \
    echo "$UNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Intel MKL
RUN apt-get install -y wget && \
    wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB && \
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB && \
    rm GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB && \
    echo "deb https://apt.repos.intel.com/oneapi all main" | tee /etc/apt/sources.list.d/oneAPI.list && \
    apt-get update && \
    apt-get install -y intel-oneapi-mkl

# Intel oneapi
RUN apt-get install -y intel-oneapi-compiler-fortran intel-oneapi-openmp intel-oneapi-mpi intel-oneapi-mpi-devel intel-oneapi-compiler-dpcpp-cpp-and-cpp-classic

# Install cmake 3.27.7 (because DIRAC requires cmake >= 3.20)
RUN mkdir -p tmp && cd tmp && wget https://github.com/Kitware/CMake/releases/download/v3.27.7/cmake-3.27.7-linux-x86_64.tar.gz && tar -xvf cmake-3.27.7-linux-x86_64.tar.gz

# Change to docker user
USER ${UNAME}
WORKDIR /workspace

# Install DIRAC
RUN git clone https://gitlab.com/dirac/dirac.git && cd dirac && git checkout -b release-23 origin/release-23 && git submodule update --init --recursive

# cmake 3.27.7
RUN echo "export PATH=/tmp/cmake-3.27.7-linux-x86_64/bin:$PATH" >> ~/.bashrc
# Intel oneapi environment
RUN echo ". /opt/intel/oneapi/setvars.sh" >> ~/.bashrc
