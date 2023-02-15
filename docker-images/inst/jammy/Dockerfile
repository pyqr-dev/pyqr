FROM ghcr.io/ijlyttle/pyqr-base:edge-jammy

# need to install as root
USER root 

# My guess is that Quarto will change most-quickly, then R, then Python 
# - putting Quarto last will save build time, as 

# install Python

# install R
RUN ansible-playbook /usr/local/ansible/local.yml --tags R

# install Quarto

# return to default user
USER docker

CMD ["bash"]