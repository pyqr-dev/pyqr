ARG IMAGE_REPO=ghcr.io/ijlyttle
ARG IMAGE_VERSION=edge-jammy
FROM ${IMAGE_REPO}/pyqr-batch:${IMAGE_VERSION}

# need to install as root
USER root 

RUN apt-get update \
  && apt-get install -y \
  zsh 

# cleanup 
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/* 

# return to default user
USER docker

CMD ["zsh"]