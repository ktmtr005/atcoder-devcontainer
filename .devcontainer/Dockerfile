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
RUN pip --default-timeout=10000 install numpy==1.24.1 \
scipy==1.10.1 \
networkx==3.0 \
sympy==1.11.1 \
sortedcontainers==2.4.0 \
more-itertools==9.0.0 \
shapely==2.0.0 \
bitarray==2.6.2 \
PuLP==2.7.0 \
mpmath==1.2.1 \
pandas==1.5.2 \
z3-solver==4.12.1.0 \
scikit-learn==1.2.0 \
ortools==9.5.2237 \
setuptools==66.0.0 \
cppyy==2.4.1 \
torch==1.13.1 \
polars==0.15.15 \
lightgbm==3.3.1 \
gmpy2==2.1.5 \
numba==0.57.0 \
git+https://github.com/not522/ac-library-python \
--break-system-packages

# コンテスト補助アプリケーションをインストール
USER root
RUN pip install online-judge-tools --break-system-packages
RUN npm install -g npm
RUN npm install -g atcoder-cli
USER ${USER}
RUN echo 'export PATH="~/.local/bin:$PATH"' >> ~/.bashrc

# Rust をインストール
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y