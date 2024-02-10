ARG VARIANT="ubuntu-22.04"
FROM mcr.microsoft.com/devcontainers/cpp:${VARIANT}

ARG CMDLINE_TOOLS_URL="https://dl.google.com/android/repository/commandlinetools-linux-9862592_latest.zip"

ENV LANG="en_US.UTF-8" \
    TZ="Asia/Taipei" \
    TERM="xterm-256color" \
    ANDROID_ARCH="android_arm64_v8a" \
    ANDROID_SDK_ROOT="/opt/android-sdk" \
    ANDROID_NDK_ROOT="/opt/android-sdk/ndk/25.2.9519653" \
    QT_VERSION="6.6.1" \
    QT_PATH="/opt/Qt" \
    Qt6_DIR="/opt/Qt/6.6.1/android_arm64_v8a" \
    QT_PLUGIN_PATH="/opt/Qt/6.6.1/android_arm64_v8a/plugins" \
    QML2_IMPORT_PATH="/opt/Qt/6.6.1/android_arm64_v8a/qml"

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
        libgl1-mesa-dev \
        libgstreamer-gl1.0-0 \
        libpulse-dev \
        libxcb-glx0 \
        libxcb-icccm4 \
        libxcb-image0 \
        libxcb-keysyms1 \
        libxcb-randr0 \
        libxcb-render-util0 \
        libxcb-render0 \
        libxcb-shape0 \
        libxcb-shm0 \
        libxcb-sync1 \
        libxcb-util1 \
        libxcb-xfixes0 \
        libxcb-xinerama0 \
        libxcb-xkb-dev \
        libxcb1 \
        libxkbcommon-dev \
        libxkbcommon-x11-0 \
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
    mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    unzip /tmp/commandlinetools-linux.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/10.0 && \
    rm -rf /tmp/commandlinetools-linux.zip && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "platform-tools" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "platforms;android-33" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "cmake;3.22.1" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.0" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.1" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.2" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;33.0.3" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "build-tools;34.0.0" && \
    echo yes | ${ANDROID_SDK_ROOT}/cmdline-tools/10.0/bin/sdkmanager "ndk;25.2.9519653"

RUN echo "==== Installing aqtinstall ====" && \
    pip install setuptools wheel py7zr==0.20.* && \
    pip install aqtinstall==3.1.*

RUN echo "==== Installing Qt ====" && \
    aqt install-qt linux desktop ${QT_VERSION} --outputdir ${QT_PATH} --modules qtcharts qtlocation qtpositioning qtspeech qt5compat qtmultimedia qtserialport qtimageformats qtshadertools qtconnectivity qtquick3d && \
    aqt install-qt linux android ${QT_VERSION} ${ANDROID_ARCH} --outputdir ${QT_PATH} --modules qtcharts qtlocation qtpositioning qtspeech qt5compat qtmultimedia qtserialport qtimageformats qtshadertools qtconnectivity qtquick3d --autodesktop && \
    export LD_LIBRARY_PATH=${QT_PATH}/${QT_VERSION}/gcc_64/lib:${QT_PATH}/${QT_VERSION}/${ANDROID_ARCH}/lib

ARG USERNAME=vscode