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
    && apt-get install -y time tzdata tree git curl \
    && apt-get install -y gcc g++ gdb python3 python3-pip pypy3
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - &&\
    apt-get install -y nodejs

# ユーザーを変更
USER ${USER}
WORKDIR /home/${USER}

# Pythonライブラリをインストール
RUN pip install numba numpy scipy scikit-learn networkx --break-system-packages

# コンテスト補助アプリケーションをインストール
USER root
RUN pip install online-judge-tools --break-system-packages
RUN npm install -g npm
RUN npm install -g atcoder-cli
USER ${USER}
RUN echo 'export PATH="~/.local/bin:$PATH"' >> ~/.bashrc

# Rust をインストール
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y