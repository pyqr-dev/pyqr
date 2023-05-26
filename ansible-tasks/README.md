# pyqr - ansible

The goal is to get your machine up-and-running the way you like; your machine could be:

  - WSL2 (Ubuntu) hosted on a PC
  - Docker container running Ubuntu
  - "bare-metal" Ubuntu machine
  - MacOS system

On the linux side, this supports Ubuntu (focal and jammy).

## Installation

As you might imagine, you need to [install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html) on your local instance (MacOS or Ubuntu).

### Ubuntu

```bash
sudo -E apt update
sudo -E apt install software-properties-common
sudo -E add-apt-repository --yes --update ppa:ansible/ansible
sudo -E apt install ansible
```

### MacOS

A couple prerequisites, `xcode-select` and `homebrew`:

```bash
xcode-select --install
```

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/fc4a19b38608451cabe6ceaa8ffb4fa1300854c8/install.sh)"
```

You can install ansible using homebrew:

```bash
brew update
brew install ansible
```

## Usage

There are two ways you can run, locally or remotely.

### Locally

If you want to run locally, install locally: 

```bash
git clone https://github.com/pyqr-dev/pyqr.git
```

Change to the directory, then:

```bash
# using example tags
ansible-playbook local.yml -K --tags "R, python, quarto"
```

### Remotely

```bash
# using example tags
ansible-pull https://github.com/pyqr-dev/pyqr.git -K --tags "R, python, quarto"
```

To install the data stack, use the tags `"R, python, quarto"`.

To install the developer stack, use the tags `"zsh, ohmyzsh, gh, stow"`.

