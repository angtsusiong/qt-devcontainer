ARG VARIANT="ubuntu-22.04"
FROM mcr.microsoft.com/devcontainers/cpp:${VARIANT}

ARG CMDLINE_TOOLS_URL="https://dl.google.com/android/repository/commandlinetools-linux-9862592_latest.zip"

ENV LANG="en_US.UTF-8" \
    TZ="Asia/Taipei" \
    TERM="xterm-256color"

RUN set -xe && \
    echo "==== Setting locale ====" && \
    update-locale LANG=${LANG} && \
    echo "==== Setting timezone ====" && \
    unlink /etc/localtime && \
    ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && \
    echo ${TZ} > /etc/timezone

RUN echo "==== Installing dependency ====" && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        binutils \
        checkinstall \
        clangd \
        clang-format \
        clang-tidy \
        clang-tools \
        dia \
        doxygen \
        espeak \
        fuse \
        gosu \
        graphviz \
        kmod \
        libespeak-dev \
        libfontconfig1 \
        libfuse2 \
        libsdl2-dev \
        libssl-dev \
        libudev-dev \
        lld \
        lldb \
        openjdk-11-jdk \
        openssl \
        patchelf \
        python3 \
        python3-pip \
        speech-dispatcher \
        xdot \
        xvfb  && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

RUN echo "==== Installing cmdline-tools 10 ====" && \
    wget --quiet ${CMDLINE_TOOLS_URL} -O /tmp/commandlinetools-linux.zip && \
    unzip /tmp/commandlinetools-linux.zip -d /opt/android-sdk/cmdline-tools && \
    mv /opt/android-sdk/cmdline-tools/cmdline-tools /opt/android-sdk/cmdline-tools/10.0 && \
    rm -rf "/tmp/commandlinetools-linux.zip" && \
    export ANDROID_SDK_ROOT="/opt/android-sdk" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "platform-tools" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "platforms;android-33" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "cmake;3.22.1" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.0" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.1" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.2" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.3" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;34.0.0" && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "ndk;25.2.9519653"

ARG USERNAME=vscode