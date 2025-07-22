# Start from official Rust slim image (Debian bookworm-based)
FROM rust:1.82-slim-bookworm

# Avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Install protoc and other dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        protobuf-compiler \
        curl \
        ca-certificates \
        git \
        build-essential \
        pkg-config \
        libssl-dev \
        sudo \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*



# Arguments
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

# Create group if GID is not in use
RUN if ! getent group "${USER_GID}" > /dev/null; then \
        groupadd --gid "${USER_GID}" "${USERNAME}"; \
    else \
        groupadd "${USERNAME}"; \
    fi && \
    # Create user if UID is not in use
    if ! id -u "${USER_UID}" > /dev/null 2>&1; then \
        useradd --uid "${USER_UID}" --gid "${USERNAME}" -m "${USERNAME}"; \
    else \
        useradd --gid "${USERNAME}" -m "${USERNAME}"; \
    fi && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}
WORKDIR /home/${USERNAME}