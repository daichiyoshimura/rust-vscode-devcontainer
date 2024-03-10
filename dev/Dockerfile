FROM rust:1.76-slim-bookworm

ARG GITCONFIG_EMAIL
ARG GITCONFIG_NAME
ARG APP_NAME

RUN sudo apt-get update & apt-get upgrade

# time *********************************************
RUN apt-get install tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
# **************************************************

# git、git-prompt、git-zsh-completion をインストールする
RUN apt-get update && apt-get install -y git

# git-prompt および git-zsh-completion を追加する
RUN apt-get update && apt-get install -y \
    zsh \
    && git clone --depth 1 https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh \
    && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc \
    && sed -i 's/robbyrussell/agnoster/' ~/.zshrc \
    && echo "source /usr/share/git/completion/git-prompt.sh" >> ~/.zshrc \
    && echo "source /usr/share/git/completion/git-completion.zsh" >> ~/.zshrc \
    && rm -rf ~/.oh-my-zsh/plugins/git \
    && git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions \
    && git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting ~/.oh-my-zsh/plugins/zsh-syntax-highlighting \
    && sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc \
    && chsh -s /bin/zsh

RUN git config --global user.email ${GITCONFIG_EMAIL} && \
    git config --global user.name ${GITCONFIG_NAME}

ENTRYPOINT /bin/zsh
# **************************************************

# https ********************************************
# 必要なパッケージをインストールする
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# ca-certificates を更新する
RUN update-ca-certificates
ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
ENV SSL_CERT_DIR=/etc/ssl/certs
# **************************************************

# the others ...
RUN apt-get update && apt-get install -y vim curl make

# rust ************************************************
# it makes faster cargo build
ENV CARGO_BUILD_TARGET_DIR=/tmp/target

# target runtime
RUN rustup target add x86_64-unknown-linux-gnu

# linter and formatter
RUN rustup component add clippy
RUN rustup component add rustfmt

# for VS Code Remote Container
RUN rustup component add rust-analysis
RUN rustup component add rust-src
RUN rustup component add rls
# ***************************************************