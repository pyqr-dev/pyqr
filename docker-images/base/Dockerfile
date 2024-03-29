ARG DIST=jammy
FROM ubuntu:${DIST}

LABEL org.opencontainers.image.source "https://github.com/pyqr-dev/pyqr" 
LABEL org.opencontainers.image.authors "Ian Lyttle <ijlyttle@me.com>" 
LABEL org.opencontainers.image.description "Foundation for GitHub Actions runner to install Python/Quarto/R"
LABEL org.opencontainers.image.licenses "MIT"

# set environment
ENV TZ=Etc/UTC
ARG DEBIAN_FRONTEND=noninteractive

# package (why)
#   sudo (other actions)
#   git (gh-pages)
#   rsync (pkgdown gh-pages deploy)
#   curl (used below)
#   wget (quarto)
#   jq (quarto)
#   gdebi-core (pandoc)
#   locales (R)
#   tzdata (R, tinytex)
#   texinfo (R, tinytex - due to setup-tinytex action)
RUN apt-get update \
  && apt-get install -y\
  sudo \ 
  git \
  rsync \
  curl \
  wget \
  jq \
  gdebi-core \
  locales \
  tzdata \
  texinfo

# python-centric packages
# ref: https://github.com/pyenv/pyenv/wiki#suggested-build-environment
RUN apt-get install -y\
  build-essential\
  libssl-dev\
  zlib1g-dev\
  libbz2-dev\
  libreadline-dev\
  libsqlite3-dev\
  libncursesw5-dev\ 
  xz-utils\
  tk-dev\ 
  libxml2-dev\ 
  libxmlsec1-dev\ 
  libffi-dev\ 
  liblzma-dev

# establish locale
COPY locale.gen /etc/locale.gen
RUN locale-gen 
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# install gh (quarto)
# ref: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-raspberry-pi-os-apt
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
  && sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && sudo apt-get update \
  && sudo apt-get install -y gh

# install rig
# ref: https://github.com/r-lib/rig/blob/main/containers/r/Dockerfile
RUN ARCH="$(arch)"; \
  if [ "$ARCH" = "aarch64" ]; then ARM64="-arm64"; else ARM64=""; fi; \
  curl -Ls https://github.com/r-lib/rig/releases/download/latest/rig-linux${ARM64}-latest.tar.gz | tar xz -C /usr/local

# install qvm
# ref: https://github.com/dpastoor/qvm/releases
RUN wget https://github.com/dpastoor/qvm/releases/download/v0.3.0/qvm_Linux_x86_64.tar.gz -O /tmp/qvm.tar.gz \
  && tar xzf /tmp/qvm.tar.gz qvm \
  && mv qvm /usr/local/bin/qvm \
  && chmod +x /usr/local/bin/qvm 

# cleanup (normally last, but installing pyenv as docker user)
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/*   

# add user `docker`
RUN useradd docker \
  && mkdir /home/docker \
  && chown docker:docker /home/docker 

# run as `docker`
USER docker
WORKDIR /home/docker 

# ====== bashrc
COPY --chown=docker:docker .bashrc .bashrc
COPY --chown=docker:docker .profile .profile
COPY --chown=docker:docker .sudo_as_admin_successful .sudo_as_admin_successful

# install pyenv (as user, `docker`)
# ref: https://github.com/pyenv/pyenv-installer#install
RUN curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

# add config to dotfiles
COPY --chown=docker:docker --chmod=555 pyenv.sh pyenv.sh
RUN cat ~/pyenv.sh >> ~/.bashrc
RUN cat ~/pyenv.sh >> ~/.profile
RUN cat ~/pyenv.sh >> ~/.zshrc
RUN cat ~/pyenv.sh >> ~/.zprofile
RUN rm pyenv.sh

COPY --chown=docker:docker --chmod=555 qvm.sh qvm.sh
RUN cat ~/qvm.sh >> ~/.bashrc
RUN cat ~/qvm.sh >> ~/.profile
RUN cat ~/qvm.sh >> ~/.zprofile
RUN cat ~/qvm.sh >> ~/.zprofile
RUN rm qvm.sh

CMD ["bash"]
