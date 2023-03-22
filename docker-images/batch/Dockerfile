ARG DIST=jammy
ARG PYTHON_VERSION=3.11.2

FROM ghcr.io/ijlyttle/pyqr-base:edge-${DIST}

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

# install 
RUN pyenv install -s ${PYTHON_VERSION}
RUN pyenv global ${PYTHON_VERSION}

# need to install as root
# USER root 

# install R

# install Quarto

# return to default user
# USER docker

CMD ["bash"]