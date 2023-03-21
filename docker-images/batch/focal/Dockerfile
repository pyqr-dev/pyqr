FROM ghcr.io/ijlyttle/pyqr-base:edge-focal

# need to install as root
USER root 

# ======= TEMPORARY ============
# go back and put into base immage
# install playbooks
COPY local.yml /usr/local/ansible/local.yml
COPY ansible-tasks/ /usr/local/ansible/ansible-tasks/

# My guess is that Quarto will change most-quickly, then R, then Python 
# - putting Quarto last will save build time, as 

# install Python
RUN ansible-playbook /usr/local/ansible/local.yml --tags pyenv
RUN ansible-playbook /usr/local/ansible/local.yml --tags python

# install R
RUN ansible-playbook /usr/local/ansible/local.yml --tags rig

# install Quarto

# return to default user
USER docker

CMD ["bash"]