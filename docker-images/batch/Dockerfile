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
ARG PAK_VERSION=devel

# install R using rig
# ref: https://github.com/r-lib/rig/blob/main/containers/r/Dockerfile
RUN ARCH="$(arch)"; \
  if [ "$ARCH" = "aarch64" ]; then ARM64="-arm64"; else ARM64=""; fi; \
  curl -Ls https://github.com/r-lib/rig/releases/download/latest/rig-linux${ARM64}-latest.tar.gz | tar xz -C /usr/local && \
  rig add ${R_VERSION} --without-pak && \
  rig default ${R_VERSION} && \
  rm -rf /tmp/rig && \
  apt-get clean

RUN versions="$(rig ls --json | grep '"name"' | sed 's/^.*\"name\": \"//' | sed 's/\",//')" && \
  for ver in $versions; do \
    R-${ver} -q -e \
    'install.packages("pak", lib = .Library, repos = sprintf("https://r-lib.github.io/p/pak/%s/%s/%s/%s", "devel", .Platform$pkgType, R.Version()$os, R.Version()$arch))'; \
  done

ENV PKG_SYSREQS=true
ENV R_PKG_SYSREQS2=true

# ====== Quarto
ARG QUARTO_VERSION=1.3.283

# install Quarto
# ref: https://github.com/analythium/quarto-docker-examples#create-a-quarto-parent-image
RUN ARCH="$(arch)"; \
  if [ "$ARCH" = "aarch64" ]; then PLATFORM="arm64"; else PLATFORM="amd64"; fi; \
  curl -o quarto.deb -L https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-${PLATFORM}.deb && \
  gdebi --non-interactive quarto.deb

# return to default user
USER docker

CMD ["bash"]