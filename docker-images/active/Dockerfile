ARG IMAGE_REPO=ghcr.io/ijlyttle
ARG IMAGE_VERSION=edge-jammy
FROM ${IMAGE_REPO}/pyqr-batch:${IMAGE_VERSION}

# need to install as root
USER root 

# zsh, stow
RUN apt-get update \
  && apt-get install -y \
  zsh \
  stow

# cleanup 
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/* 

# return to default user
USER docker

# ohmyzsh ref: https://ohmyz.sh/#install
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

CMD ["zsh"]