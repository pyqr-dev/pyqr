ARG IMAGE_VERSION=edge-jammy
FROM ghcr.io/ijlyttle/pyqr-base:${IMAGE_VERSION}}

# ====== Python
ARG PYTHON_VERSION=3.11.2

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

# ====== Quarto
ARG QUARTO_VERSION=pre_release

# install Quarto
# ref: https://github.com/ijlyttle/quig
RUN quig add ${QUARTO_VERSION}

# return to default user
USER docker

CMD ["bash"]