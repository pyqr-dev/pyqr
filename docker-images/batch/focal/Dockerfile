FROM ghcr.io/ijlyttle/pyqr-base:edge-focal

# install pyenv
# ref: https://github.com/pyenv/pyenv-installer#install
RUN curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

# add config to dotfiles
COPY --chmod=guo+x docker-images/batch/pyenv.sh pyenv.sh
RUN cat ~/pyenv.sh >> ~/.bashrc
RUN cat ~/pyenv.sh >> ~/.profile
RUN cat ~/pyenv.sh >> ~/.zshrc
RUN cat ~/pyenv.sh >> ~/.zprofile

# install 
# RUN pyenv install -s 3.11.2 
# RUN pyenv global 3.11.2

# check
# RUN pyenv versions

# need to install as root
# USER root 

# install R

# install Quarto

# return to default user
# USER docker

CMD ["bash"]