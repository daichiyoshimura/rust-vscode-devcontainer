FROM rust:1.76-slim-bookworm

ARG GITCONFIG_EMAIL
ARG GITCONFIG_NAME
ARG REPO_URL

# Install necessary packages
RUN apt-get update && \
    apt-get install -y tzdata git zsh vim curl make wget ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set timezone to Asia/Tokyo
RUN ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# Configure Git and Zsh
RUN git config --global user.email "${GITCONFIG_EMAIL}" && \
    git config --global user.name "${GITCONFIG_NAME}" && \
    sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# Clone repository
RUN git clone ${REPO_URL} /rust/src/app

# Set working directory
WORKDIR /rust/src/app

# Install Rust tools
RUN rustup target add x86_64-unknown-linux-gnu && \
    rustup component add clippy rustfmt rust-analysis rust-src rls

# Set environment variables
ENV CARGO_BUILD_TARGET_DIR=/tmp/target

# Set default entrypoint
ENTRYPOINT ["/bin/zsh"]
