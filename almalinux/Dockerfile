FROM almalinux:8

# For intel-oneapi
RUN { \
    echo "[oneAPI]"; \
    echo "name=Intel® oneAPI repository"; \
    echo "baseurl=https://yum.repos.intel.com/oneapi"; \
    echo "enabled=1"; \
    echo "gpgcheck=1"; \
    echo "repo_gpgcheck=1"; \
    echo "gpgkey=https://yum.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB"; \
    } >> /tmp/oneAPI.repo

RUN mv /tmp/oneAPI.repo /etc/yum.repos.d

# procs-ng is for ps command (required by the /opt/intel/oneapi/setvars.sh script)
# gcc-c++ is shomehow required for the intel-oneapi icc compiler
# ref: https://lists.openhpc.community/g/users/topic/intel_compilers_require_gcc/12127197#:~:text=%23%20yum%20%2D%2Dinstallroot%3D/opt/ohpc/admin/images/centos7.4/%20install%20gcc%2Dc%2B%2B
RUN yum update -y && yum install -y cmake python3 python3-pip git wget sudo which procps-ng gcc-c++

# Create docker user
ENV UNAME=docker GID=1000 UID=1000
# Add sudo group
RUN groupadd -g $GID -o $UNAME && useradd -m -u $UID -g $GID -o -s /bin/bash $UNAME && \
    echo "$UNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN usermod -aG wheel ${UNAME}

# Intel oneapi
RUN yum install -y intel-oneapi-mkl intel-oneapi-compiler-fortran intel-oneapi-openmp intel-oneapi-mpi intel-oneapi-mpi-devel intel-oneapi-compiler-dpcpp-cpp-and-cpp-classic

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
