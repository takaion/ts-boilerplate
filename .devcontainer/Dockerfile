FROM node:26-trixie

ARG USERNAME=node
ENV TZ=Asia/Tokyo

ARG PNPM_VERSION
ARG TYPESCRIPT_VERSION

RUN apt update && apt install --no-install-recommends -y sudo && apt clean && rm -rf /var/lib/apt/lists/*

# javascript-node と同様にする
# https://github.com/devcontainers/images/blob/main/src/javascript-node/.devcontainer/Dockerfile
# ===== javascript-node =====
ARG NPM_GLOBAL=/usr/local/share/npm-global
ENV PATH=${NPM_GLOBAL}/bin:${PATH}
ENV LANG=C.UTF-8

RUN \
    # Configure global npm install location, use group to adapt to UID/GID changes
    if ! cat /etc/group | grep -e "^npm:" > /dev/null 2>&1; then groupadd -r npm; fi \
    && usermod -a -G npm ${USERNAME} \
    && umask 0002 \
    && mkdir -p ${NPM_GLOBAL} \
    && touch /usr/local/etc/npmrc \
    && chown ${USERNAME}:npm ${NPM_GLOBAL} /usr/local/etc/npmrc \
    && chmod g+s ${NPM_GLOBAL} \
    && npm config -g set prefix ${NPM_GLOBAL} \
    && su ${USERNAME} -c "npm config -g set prefix ${NPM_GLOBAL}" \
    # Install eslint
    && su ${USERNAME} -c "umask 0002 && npm install -g eslint" \
    && npm cache clean --force > /dev/null 2>&1
# ====================

# pnpm 有効化
RUN npm install -g corepack
USER $USERNAME
RUN corepack enable && corepack install -g pnpm@12.16.1
