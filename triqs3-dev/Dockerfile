FROM ubuntu:focal
ARG LLVM=9

# This platform includes dependencies for building docs
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
      clang-${LLVM} \
      cmake \
      g++ \
      gfortran \
      git \
      vim \
      lldb-${LLVM} \
      hdf5-tools \
      libmkl-full-dev \ 
      libboost-dev \
      libclang-${LLVM}-dev \
      libc++-${LLVM}-dev \
      libc++abi-${LLVM}-dev \
      libfftw3-dev \
      libnfft3-dev \
      libgfortran4 \
      libgmp-dev \
      libhdf5-dev \
      libopenmpi-dev \
      openmpi-bin \
      openmpi-common \
      openmpi-doc \
      python3-clang-${LLVM} \
      python3-dev \
      python3-mako \
      python3-numpy \
      python3-scipy \
      python3-matplotlib \
      python3-mpi4py \
      python3-pip \
      python3-sphinx \
      python3-nbsphinx \
      git \
      curl \
      ca-certificates \
      openssl \
      wget \
      rsync \
      python-configparser \ 
      libomp-dev \
      nano \
      sudo \
      && \
      apt-get autoremove --purge -y && \
      apt-get autoclean -y && \
      rm -rf /var/cache/apt/* /var/lib/apt/lists/*
     
ENV PYTHON_VERSION=3.8 \
    CC=clang-${LLVM} CXX=clang++-${LLVM} CXXFLAGS="-stdlib=libc++"
RUN update-alternatives --install /usr/bin/clang clang /usr/bin/clang-${LLVM} 60 --slave /usr/bin/clang++ clang++ /usr/bin/clang++-${LLVM} --slave /usr/bin/clang-cpp clang-cpp /usr/bin/clang-cpp-${LLVM}

ENV OMP_NUM_THREADS=1
ENV MKL_NUM_THREADS=1

# triqs
RUN cd / && mkdir -p triqs && mkdir -p source

ENV CPATH=/triqs/include:${CPATH} \
    PATH=/triqs/bin:${PATH} \
    LIBRARY_PATH=/triqs/lib:${LIBRARY_PATH} \
    LD_LIBRARY_PATH=/triqs/lib:${LD_LIBRARY_PATH} \
    PYTHONPATH=/triqs/lib/python3.8/site-packages:${PYTHONPATH} \
    CMAKE_PREFIX_PATH=/triqs/share/cmake:${CMAKE_PREFIX_PATH} \
    TRIQS_ROOT=/triqs

RUN cd /source && git clone -b unstable https://github.com/TRIQS/triqs triqs.src \
    && mkdir -p triqs.build && cd triqs.build \
    && cmake ../triqs.src -DCMAKE_INSTALL_PREFIX=/triqs -DMPIEXEC_PREFLAGS='--allow-run-as-root' -DBLAS_LIBRARIES=/lib/x86_64-linux-gnu/libmkl_rt.so \
    && make -j4 && ctest -j4 && make install

# dft_tools
RUN cd /source && git clone -b unstable https://github.com/TRIQS/dft_tools.git dft_tools.src \
    && mkdir -p dft_tools.build && cd dft_tools.build \
    && cmake ../dft_tools.src -DMPIEXEC_PREFLAGS='--allow-run-as-root' \
    && make -j4 && ctest -j4 && make install

# cthyb
RUN cd /source && git clone -b unstable https://github.com/TRIQS/cthyb.git cthyb.src \
    && mkdir -p cthyb.build && cd cthyb.build \
    && cmake ../cthyb.src -DMPIEXEC_PREFLAGS='--allow-run-as-root'  \
    && make -j4 && ctest -j4 && make install

# download solidmft inside container
RUN cd / && git clone https://github.com/materialstheory/soliDMFT.git

# remove source
RUN cd / && rm -rf source

# create a useful work dir
RUN cd / && mkdir work && cd work
ENV OMPI_MCA_btl_vader_single_copy_mechanism=none

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN ["chmod", "+x", "/usr/local/bin/entrypoint.sh"]

RUN `ln -s /usr/bin/python3 /usr/bin/python`

# change user and group id to match host machine if options are passed accordingly
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

