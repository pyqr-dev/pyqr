ARG IMAGE_REPO=ghcr.io/pyqr-dev
ARG IMAGE_VERSION=edge-jammy
FROM ${IMAGE_REPO}/pyqr-batch:${IMAGE_VERSION}

# need to install as root
USER root 

# zsh, stow, vim
RUN apt-get update \
  && apt-get install -y \
  zsh \
  stow \
  vim

# ansible
RUN apt-get install -y software-properties-common \
  && add-apt-repository --yes --update ppa:ansible/ansible \
  && apt-get install -y ansible

# cleanup 
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/* 

# return to default user
USER docker

# ohmyzsh ref: https://ohmyz.sh/#install
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# new .zshrc: append previous contents, e.g. pyenv
RUN cat ~/.zshrc.pre-oh-my-zsh >> ~/.zshrc

# zsh-autocomplete ref: https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh
RUN git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

CMD ["zsh"]
