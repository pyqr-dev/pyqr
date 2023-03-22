ARG DIST=jammy
FROM ghcr.io/ijlyttle/pyqr-base:edge-${DIST}

# ====== Python
ARG PYTHON_VERSION=3.11.2

# install pyenv
# ref: https://github.com/pyenv/pyenv-installer#install
RUN curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

# add config to dotfiles
COPY --chown=docker:docker --chmod=555 pyenv.sh pyenv.sh
RUN cat ~/pyenv.sh >> ~/.bashrc
RUN cat ~/pyenv.sh >> ~/.profile
RUN cat ~/pyenv.sh >> ~/.zshrc
RUN cat ~/pyenv.sh >> ~/.zprofile
RUN rm pyenv.sh

# install Python
RUN .pyenv/bin/pyenv install -s ${PYTHON_VERSION}
RUN .pyenv/bin/pyenv global ${PYTHON_VERSION}

# need to install as root
USER root 

# ====== R
ARG R_VERSION=4.2.3
ARG PAK_VERSION devel

# install R using rig
# ref: https://github.com/r-lib/rig/blob/main/containers/r/Dockerfile
RUN ARCH="$(arch)"; \
    if [ "$ARCH" = "aarch64" ]; then ARM64="-arm64"; else ARM64=""; fi; \
    curl -Ls https://github.com/r-lib/rig/releases/download/latest/rig-linux${ARM64}-latest.tar.gz | tar xz -C /usr/local && \
    rig add release --without-pak && \
    rig default release && \
    rm -rf /tmp/rig && \
    apt-get clean

RUN versions="$(rig ls --json | grep '"name"' | sed 's/^.*\"name\": \"//' | sed 's/\",//')" && \
    for ver in $versions; do \
      R-${ver} -q -e \
      'install.packages("pak", lib = .Library, repos = sprintf("https://r-lib.github.io/p/pak/%s/%s/%s/%s", "devel", .Platform$pkgType, R.Version()$os, R.Version()$arch))'; \
    done

ENV PKG_SYSREQS=true
ENV R_PKG_SYSREQS2=true

# install Quarto

# return to default user
USER docker

CMD ["bash"]