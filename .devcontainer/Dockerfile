FROM ubuntu:latest

# インタラクティブモードにならないようにする
ARG DEBIAN_FRONTEND=noninteractive
ARG TZ=Asia/Tokyo

# ユーザーを作成
ARG USER="vscode"
RUN groupadd --gid 1000 ${USER} \
    && useradd --uid 1000 --gid 1000 -m ${USER}

# C++, Python3, PyPy3, Rustのインストール
RUN apt-get update \
    && apt-get install -y time tzdata tree git curl \
    && apt-get install -y gcc g++ gdb python3 python3-pip pypy3 rust-all nodejs npm 

# ユーザーを変更
USER ${USER}
WORKDIR /home/${USER}

# Pythonライブラリをインストール
RUN pip install numba numpy scipy scikit-learn networkx

# コンテスト補助アプリケーションをインストール
RUN pip install online-judge-tools
RUN mkdir /home/${USER}/.npm-global \
    && npm config set prefix '~/.npm-global'
RUN echo "export PATH=~/.npm-global/bin:$PATH" >> /home/${USER}/.profile \
    && . /home/${USER}/.profile
RUN npm install -g atcoder-cli
