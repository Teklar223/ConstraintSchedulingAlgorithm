FROM mcr.microsoft.com/devcontainers/universal:2-linux

ENV VCPKG_ROOT /com.docker.devenvironments.code/vcpkg

# Configure apt and install packages
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    #
    # Install C++ tools and Git
    && apt-get -y install build-essential gdb cmake cppcheck git zip unzip \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Install vcpkg dependency manager
RUN git clone https://github.com/Microsoft/vcpkg ${VCPKG_ROOT} \
    && cd ${VCPKG_ROOT} \
    # This is (for now) the way vcpkg handles versioning: freezing the cloned repo at a specific commit!
    # See https://github.com/microsoft/vcpkg/issues/1681.
    && git reset --hard 077c0746be3773fdd443af52cbe5c3cc26379a15 \
    && ./bootstrap-vcpkg.sh

ENV PATH "${VCPKG_ROOT}:${PATH}"

# Install C++ dependencies using vcpkg
RUN vcpkg integrate install \
    && vcpkg install gtest