# pyqr - docker

This is yet another set of container images, which is especially egregious given all the existing well-supported, well-established container projects. I don't know of a container project that does quite this, so I'm using this as a learning experience. 

It offers containers at three levels:

- **base**: designed for GitHub Actions, can run `actions/setup-python`, etc., or can use `pyenv`, `rig`, etc. 
- **batch**: designed for GitHub Actions, but has "standard" versions of Python, Quarto, and R already installed.
- **active**: designed for interactive use, has zsh, stow, ansible, etc. installed.

At present, I'm getting things to work, hence the `edge` tag. At some point, I'll implement proper versioning.

## Levels

### `base`

The purpose of  is to provide a foundation upon which these GitHub Actions can be run:

- [actions/setup-python](https://github.com/actions/setup-python)
- [quarto-dev/quarto-actions](https://github.com/quarto-dev/quarto-actions) (`setup`, `render`, `publish`)
- [r-lib/actions](https://github.com/r-lib/actions) (`setup-r`, `setup-r-dependencies`, `setup-renv`, `check-r-package`, `setup-pandoc`, `setup-tinytex`)

In a GitHub actions workflow, you could use this container using:

```yaml
jobs:
  <name-of-job>:
    runs-on: <name-of-runner>
    container:
      image: ghcr.io/pyqr-dev/pyqr-base:edge-jammy
```

These environment variables are set for `base`:

```dockerfile
ENV TZ=Etc/UTC
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
```

### `batch`

This installs "standard" versions of Python, Quarto, and R; see the [Dockerfile](batch/Dockerfile) for details. 

These environment variables are set for `batch`:

```dockerfile
ENV PKG_SYSREQS=true
ENV R_PKG_SYSREQS2=true
```

### `active`

This is designed for interactive development. 
It includes:

 - [zsh](https://www.zsh.org/)
 - [oh-my-zsh](https://ohmyz.sh/)
 - [Ansible](https://www.ansible.com/)
 - [GNU Stow](https://www.gnu.org/software/stow/)

 This can support keeping a set of "public" dotfiles and an encrypted set of "secret" dotfiles containing tokens, etc., using [Ansible Vault](https://docs.ansible.com/ansible/2.8/user_guide/vault.html#).

## Building

For example, from `docker-images/active`:

```bash
docker buildx build . --tag local/pyqr-active:edge-jammy --build-arg IMAGE_VERSION=edge-jammy
```

## Usage

TODO: note [MacOS M1/M2 emulation challenge](https://github.com/docker/roadmap/issues/384), i.e. `amd64` vs. `arm64`.

```bash
docker run -it local/pyqr-active:edge-jammy
```

If you want to run as root, perhaps to install additional software:

```bash
docker run -it --user root local/pyqr-active:edge-jammy
```

### Remote

```bash
docker pull ghcr.io/pyqr-dev/pyqr-active:edge-jammy
docker tag ghcr.io/pyqr-dev/pyqr-active:edge-jammy pyqr-active:edge-jammy
```

```bash
docker run -it --user root pyqr-active:edge-jammy
```

## Acknowledgements

Once I started this, I came across [Posit's r-docker project](https://github.com/rstudio/r-docker), whose purpose is tantalizingly close to the purpose of this project:

  - r-docker is meant to support installation of R.
  - this project is meant to support installation of Python, Quarto, and R.

Given how close these are, and that Posit is *way* more capable than I am, we should not be surprised if this project has a short life.

This work also draws from these inspirations:

- Brian Holt's [Complete Introduction to Containers (feat. Docker)](https://frontendmasters.com/courses/complete-intro-containers/), offered by Frontend Masters.
- Danielle Navarro's blog post, [*Playing with Docker*](https://blog.djnavarro.net/posts/2023-01-01_playing-with-docker/), and its [accompanying repository](https://github.com/djnavarro/arch-r).
- The [rocker-org/ubuntu-lts](https://github.com/rocker-org/ubuntu-lts) set of Docker images, maintained by Dirk Eddelbuettel et al.
- Gábor Csárdi wrote a [Docker image for rig](https://github.com/r-lib/rig/blob/main/containers/r/Dockerfile), the image here for R-installation is based on that.
- Peter Solymos wrote some [Docker images for Quarto](https://github.com/analythium/quarto-docker-examples). from which this work takes inspiration.
