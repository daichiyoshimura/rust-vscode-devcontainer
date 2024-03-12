FROM rust:1.76-slim-bookworm

ARG GITCONFIG_EMAIL
ARG GITCONFIG_NAME
ARG APP_NAME

RUN apt-get update && \
    apt-get install -y tzdata git zsh vim curl make wget ca-certificates && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    git clone --depth 1 https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh && \
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc && \
    git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions && \
    git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting ~/.oh-my-zsh/plugins/zsh-syntax-highlighting && \
    sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc && \
    chsh -s /bin/zsh && \
    git config --global user.email ${GITCONFIG_EMAIL} && \
    git config --global user.name ${GITCONFIG_NAME} && \
    wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.zsh -O ~/.git-completion.zsh && \
    rustup target add x86_64-unknown-linux-gnu && \
    rustup component add clippy rustfmt rust-analysis rust-src rls

ENV CARGO_BUILD_TARGET_DIR=/tmp/target

RUN mkdir -p /rust/src/${APP_NAME}
WORKDIR /rust/src/${APP_NAME}

ENTRYPOINT ["/bin/zsh"]
