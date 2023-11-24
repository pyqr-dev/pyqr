ARG IMAGE_REPO=ghcr.io/pyqr-dev
ARG IMAGE_VERSION=edge-jammy
FROM ${IMAGE_REPO}/pyqr-base:${IMAGE_VERSION}

# ====== Python
ARG PYTHON_VERSION=3.11

# install Python
RUN .pyenv/bin/pyenv install -s ${PYTHON_VERSION}
RUN .pyenv/bin/pyenv global ${PYTHON_VERSION}

# need to install as root
USER root 

# ====== R
ARG R_VERSION=release
ARG PAK_VERSION=devel

# install R using rig
# ref: https://github.com/r-lib/rig/blob/main/containers/r/Dockerfile
RUN rig add ${R_VERSION} --without-pak && \
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

# return to default user
USER docker

# ====== Quarto
ARG QUARTO_VERSION=release

# install Quarto
# ref: https://github.com/dpastoor/qvm#installing-versions
RUN qvm install ${QUARTO_VERSION} \
  && qvm use ${QUARTO_VERSION}

CMD ["bash"]