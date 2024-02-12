FROM debian:bookworm-slim

# インタラクティブモードにならないようにする
ARG DEBIAN_FRONTEND=noninteractive
ARG TZ=Asia/Tokyo

# ユーザーを作成
ARG USER="vscode"
RUN groupadd --gid 1000 ${USER} \
    && useradd --uid 1000 --gid 1000 -m ${USER}

# C++, Python3, PyPy3 のインストール
RUN apt-get update \
    && apt-get install -y time tzdata tree git curl wget unzip libssl-dev\
    && apt-get install -y gcc g++ gdb python3 python3-pip pypy3
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - &&\
    apt-get install -y nodejs

# ユーザーを変更
USER ${USER}
WORKDIR /home/${USER}

# Pythonライブラリをインストール
COPY requirements.txt /home/${USER}/
RUN pip --default-timeout=100000 install -r /home/${USER}/requirements.txt --break-system-packages

# コンテスト補助アプリケーションをインストール
USER root
RUN pip install online-judge-tools online-judge-template-generator --break-system-packages
RUN npm install -g npm
RUN npm install -g atcoder-cli
USER ${USER}
RUN echo 'export PATH="~/.local/bin:$PATH"' >> ~/.bashrc

# Rust をインストール
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# Atcoder Library をインストール
USER ${USER}
RUN mkdir /home/${USER}/.temp \
&& wget -P /home/${USER}/.temp/ https://img.atcoder.jp/practice2/ac-library.zip \
&& unzip /home/${USER}/.temp/*.zip -d /home/${USER}/.temp \
&& mkdir -p /home/${USER}/library/atcoder \
&& cp -r /home/${USER}/.temp/atcoder /home/${USER}/library/ \
&& cp /home/${USER}/.temp/expander.py /home/${USER}/library

# cargo-generate をインストール
# RUN OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu OPENSSL_INCLUDE_DIR=/usr/include/openssl  OPENSSL_DIR=/usr/bin /home/${USER}/.cargo/bin/cargo install cargo-generate

# cargo-compete をインストール
RUN OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu OPENSSL_INCLUDE_DIR=/usr/include/openssl  OPENSSL_DIR=/usr/bin /home/${USER}/.cargo/bin/cargo install cargo-compete

RUN echo 'export OPENSSL_LIB_DIR=/usr/lib/x86_64-linux-gnu' >> ~/.bashrc \
&& echo 'export OPENSSL_INCLUDE_DIR=/usr/include/openssl' >> ~/.bashrc \
&& echo 'export OPENSSL_DIR=/usr/bin' >> ~/.bashrc 
