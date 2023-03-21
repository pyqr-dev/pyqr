FROM ubuntu:focal

LABEL org.opencontainers.image.source "https://github.com/ijlyttle/pyqr" 
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

# establish locale
COPY docker-images/base/locale.gen /etc/locale.gen
RUN locale-gen 
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# install ansible
# ref: https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
RUN apt-get install -y software-properties-common \
  && add-apt-repository -y --update ppa:ansible/ansible \
  && apt-get install -y ansible

# install playbooks
RUN mkdir /usr/local/ansible
COPY local.yml /usr/local/ansible/local.yml
COPY ansible-tasks/ /usr/local/ansible/ansible-tasks/

# install gh (quarto)
RUN ansible-playbook /usr/local/ansible/local.yml --tags gh

# cleanup
RUN apt-get clean \
  && rm -rf /var/lib/apt/lists/*   

# add user `docker`
RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker 

# run as `docker`
USER docker
WORKDIR /home/docker

CMD ["bash"]