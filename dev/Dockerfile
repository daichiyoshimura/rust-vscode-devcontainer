FROM rust:1.76-slim-bookworm

ARG GITCONFIG_EMAIL
ARG GITCONFIG_NAME
ARG APP_NAME

RUN apt-get update && apt-get upgrade -y

# タイムゾーンを設定
RUN apt-get install -y tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# git、zshをインストールし、Oh My Zshをセットアップ
RUN apt-get install -y git zsh && \
    git clone --depth 1 https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh && \
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc && \
    sed -i 's/robbyrussell/agnoster/' ~/.zshrc && \
    git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions && \
    git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting ~/.oh-my-zsh/plugins/zsh-syntax-highlighting && \
    sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc && \
    chsh -s /bin/zsh

# Gitのユーザー情報を設定
RUN git config --global user.email ${GITCONFIG_EMAIL} && \
    git config --global user.name ${GITCONFIG_NAME}

# デフォルトのエントリーポイントをzshに設定
ENTRYPOINT /bin/zsh

# 必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/* && \
    update-ca-certificates && \
    export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt && \
    export SSL_CERT_DIR=/etc/ssl/certs

# その他の依存パッケージをインストール
RUN apt-get update && apt-get install -y vim curl make wget

# Gitの補完スクリプトをダウンロードしてコピー
RUN wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.zsh -O ~/.git-completion.zsh

# rust関連の設定
ENV CARGO_BUILD_TARGET_DIR=/tmp/target
RUN rustup target add x86_64-unknown-linux-gnu && \
    rustup component add clippy && \
    rustup component add rustfmt && \
    rustup component add rust-analysis && \
    rustup component add rust-src && \
    rustup component add rls
