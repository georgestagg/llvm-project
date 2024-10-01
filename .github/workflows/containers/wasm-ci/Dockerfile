ARG BASE=ubuntu:24.04
FROM $BASE
ENV DEBIAN_FRONTEND=noninteractive TZ=UTC

RUN apt-get update && apt-get -y install --no-install-recommends \
    build-essential \
    ca-certificates \
    clang \
    cmake \
    curl \
    gfortran \
    gh \
    git \
    gnupg \
    gperf \
    jq \
    libbz2-dev \
    libcurl4-openssl-dev \
    libglib2.0-dev-bin \
    liblzma-dev \
    libpcre2-dev \
    libssl-dev \
    libxml2-dev \
    libz-dev \
    lld \
    ninja-build \
    pkg-config \
    python3 \
    python3-setuptools \
    quilt \
    sqlite3 \
    sudo \
    tzdata \
    unzip \
    wget

RUN curl -O -L https://github.com/georgestagg/llvm-project/archive/refs/heads/wasm-ci.tar.gz && tar -xf wasm-ci.tar.gz

RUN mkdir build
RUN cmake -G Ninja -S llvm-project-wasm-ci/llvm -B build \
  -DCMAKE_INSTALL_PREFIX=/opt/llvm \
  -DCMAKE_BUILD_TYPE=MinSizeRel \
  -DLLVM_TARGETS_TO_BUILD="host;WebAssembly" \
  -DLLVM_ENABLE_PROJECTS="clang;lld;libc" \
  -DCMAKE_C_COMPILER=clang \
  -DCMAKE_CXX_COMPILER=clang++ \
  -DLLVM_USE_LINKER=lld

RUN cmake --build build
RUN cmake --build build -t install

FROM $BASE
ENV DEBIAN_FRONTEND=noninteractive TZ=UTC
COPY --from=0 /opt/llvm /opt/llvm
SHELL ["/bin/bash", "-c"]
