FROM rust:1.76-slim-bookworm

# Install necessary packages
RUN apt-get update && \
    apt-get install -y tzdata git zsh vim curl make wget ca-certificates && \
    update-ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set timezone to Asia/Tokyo
RUN ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# Configure Git and Zsh
RUN sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# Set working directory
WORKDIR /usr/src/app

# Install Rust tools
RUN rustup target add x86_64-unknown-linux-gnu && \
    rustup component add clippy rustfmt rust-analysis rust-src rls

# Set environment variables
ENV CARGO_BUILD_TARGET_DIR=/tmp/target

# Set default entrypoint
ENTRYPOINT ["/bin/zsh"]
