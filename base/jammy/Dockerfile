FROM ubuntu:jammy

LABEL org.opencontainers.image.source "https://github.com/ijlyttle/pyqr" 
LABEL org.opencontainers.image.authors "Ian Lyttle <ijlyttle@me.com>" 
LABEL org.opencontainers.image.description "Foundation for GitHub Actions runner to install Python/Quarto/R"
LABEL org.opencontainers.image.licenses "MIT"

# set environment
ENV TZ=Etc/UTC
ENV DEBIAN_FRONTEND=noninteractive

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

# gh cli (quarto), ref: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-raspberry-pi-os-apt
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
  && chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && apt-get update \
  && apt-get install gh -y

# establish locale
COPY locale.gen /etc/locale.gen
RUN locale-gen 
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# cleanup
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/*   

CMD ["bash"]