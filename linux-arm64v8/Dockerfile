FROM arm64v8/centos:7
LABEL maintainer="Lovell Fuller <npm@lovell.info>"

# Create CentOS 7 (glibc 2.17) container suitable for building Linux ARM64v8-A binaries

# Path settings
ENV \
  RUSTUP_HOME="/usr/local/rustup" \
  CARGO_HOME="/usr/local/cargo" \
  PATH="/usr/local/cargo/bin:/opt/rh/devtoolset-10/root/usr/bin:/opt/rh/rh-python38/root/usr/bin:/opt/rh/rh-python38/root/usr/local/bin:$PATH"

# Build dependencies
RUN \
  yum update -y && \
  yum install -y epel-release centos-release-scl && \
  yum group install -y "Development Tools" && \
  yum install -y --setopt=tsflags=nodocs \
    advancecomp \
    brotli \
    cmake3 \
    # FIXME: Update to devtoolset-11, see: https://bugs.centos.org/view.php?id=18393
    devtoolset-10-gcc \
    devtoolset-10-gcc-c++ \
    gperf \
    jq \
    nasm \
    rh-python38 \
    && \
  curl https://sh.rustup.rs -sSf | sh -s -- -y \
    --no-modify-path \
    --profile minimal \
    --default-host aarch64-unknown-linux-gnu \
    && \
  ln -s /usr/bin/cmake3 /usr/bin/cmake && \
  pip3 install meson ninja

# Compiler settings
ENV \
  PKG_CONFIG="pkg-config --static" \
  PLATFORM="linux-arm64v8" \
  FLAGS="-march=armv8-a" \
  MESON="--cross-file=/root/meson.ini"

COPY Toolchain.cmake /root/
COPY meson.ini /root/
