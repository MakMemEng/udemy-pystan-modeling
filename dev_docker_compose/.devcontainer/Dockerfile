# ベースイメージの指定
FROM ubuntu:20.04 AS builder

# パッケージのインストール
RUN apt-get update && \
    apt-get install -y \
        python3.10 \
        python3.10-dev \
        python3-pip \
        build-essential \
        wget \
        libssl-dev \
        libffi-dev \
        libcurl4-openssl-dev \
        libxml2-dev \
        libxmlsec1-dev \
        git \
        curl

# 必要なPythonパッケージのインストール
RUN pip3 install poetry
# RUN pip3 install pystan==2.18

RUN git clone https://github.com/stan-dev/httpstan && \
    cd httpstan && \
    poetry build && \
    pip3 install --prefix=/install dist/*.whl

# ソースコードのコピー
COPY . /playground

# パッケージのビルド
WORKDIR /playground
# Install project dependencies
RUN poetry config virtualenvs.create false \
    && poetry config installer.modern-installation false \
    && poetry install
# RUN poetry build && \
#     pip3 install --prefix=/install dist/*.whl

# 実行イメージの作成
FROM ubuntu:20.04

# ビルド済みのパッケージのコピー
COPY --from=builder /install /usr/local

# 必要なパッケージのインストール
RUN apt-get update && apt-get install -y \
    libssl-dev \
    libffi-dev \
    libxml2-dev \
    libxslt-dev \
    libpq-dev

# ポートの公開
EXPOSE 8000