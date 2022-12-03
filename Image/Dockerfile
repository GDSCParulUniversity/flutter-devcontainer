ARG VARIENT="ubuntu-22.04"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIENT}

ENV DEVCONTAINER="true"

# setup android sdk
RUN apt clean && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install git \
    && apt-get -y install clang cmake ninja-build pkg-config \
    && apt-get clean -y

USER vscode

RUN export HOME="/home/vscode"
ARG HOME="/home/vscode"
ENV FLUTTER_HOME="${HOME}/.local/flutter"
ENV ANDROID_HOME=/home/vscode/.local
ENV ANDROID_SDK_ROOT=/home/vscode/.local
ENV CMDLINE_HOME="${HOME}/.local/cmdline-tools"
ENV CMDLINE="${HOME}/.local/cmdline-tools/cmdline-tools/bin"

# setup flutter sdk
ENV PATH=${PATH}:${FLUTTER_HOME}/flutter/bin:${CMDLINE}


# setup command line tools
RUN ls -la ~ && whoami && sudo mkdir -pv ${CMDLINE_HOME} && cd ${CMDLINE_HOME} \
    && sudo wget https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip \
    && sudo unzip commandlinetools-linux-8512546_latest.zip \
    && sudo rm commandlinetools-linux-8512546_latest.zip

RUN ls -la ${HOME}/ && ls -la ${CMDLINE_HOME} && echo ${CMDLINE_HOME} && sudo chown -Rv vscode:vscode ${HOME}/.local
RUN sudo chmod +r+w+x -Rv ${HOME}/.local

# Install cmdlinetools and sdk
RUN sudo apt-get install openjdk-11-jdk --no-install-recommends -y
RUN yes | sdkmanager --install "build-tools;29.0.0" "platforms;android-29" "platform-tools" "cmdline-tools;latest"

# setup flutter
RUN mkdir -pv ${FLUTTER_HOME} \
    && cd ${FLUTTER_HOME} \
    && git clone -b master https://github.com/flutter/flutter \
    && flutter precache

RUN flutter config --android-sdk "/home/vscode/.local"
RUN yes | flutter doctor --android-licenses

# Cleanup
RUN sudo apt-get clean -y && sudo rm -rf /var/lib/apt/lists/*
