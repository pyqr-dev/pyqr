# pyqr - docker

The purpose of this image is to provide a foundation upon which these GitHub Actions can be run:

- [actions/setup-python](https://github.com/actions/setup-python)
- [quarto-dev/quarto-actions](https://github.com/quarto-dev/quarto-actions) (`setup`, `render`, `publish`)
- [r-lib/actions](https://github.com/r-lib/actions) (`setup-r`, `setup-r-dependencies`, `setup-renv`, `check-r-package`, `setup-pandoc`, `setup-tinytex`)

In a GitHub actions workflow, you could use this container using:

```yaml
jobs:
  <name-of-job>:
    runs-on: <name-of-runner>
    container:
      image: ghcr.io/ijlyttle/pyqr-base:edge-focal
```

At present, I'm getting things to work, hence the `edge` tag. At some point, I'll implement proper versioning.

These environment variables are set for `base`:

```dockerfile
ENV TZ=Etc/UTC
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
```

## Building

For example, from `docker-images/base`:

```bash
docker buildx build --tag base-jammy .
```

```bash
docker buildx build --tag base-focal --build-arg DIST=focal .
```

## Usage

Following the [rocker project](https://github.com/rocker-org/rocker), this container creates a default user `docker`. 
You start with a `bash` shell in the `/home/docker` directory:

```
docker run -it base-jammy
```

If you want to run as root, perhaps to install additional software:

```
docker run -it --user root base-jammy
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
