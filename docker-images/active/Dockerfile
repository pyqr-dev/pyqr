ARG IMAGE_REPO=ghcr.io/ijlyttle
ARG IMAGE_VERSION=edge-jammy
FROM ${IMAGE_REPO}/pyqr-batch:${IMAGE_VERSION}

# need to install as root
USER root 

# return to default user
USER docker

CMD ["bash"]