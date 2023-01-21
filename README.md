# pyqr

The purpose of this (set of) container(s) is to provide a foundation upon which these GitHub Actions can be run:

- [actions/setup-python](https://github.com/actions/setup-python)
- [quarto-dev/quarto-actions](https://github.com/quarto-dev/quarto-actions) (`setup`, `render`, `publish`)
- [r-lib/actions](https://github.com/r-lib/actions) (`setup-r`, `setup-r-dependencies`, `setup-renv`, `check-r-package`, `setup-pandoc`, `setup-tinytex`)

In a GitHub actions workflow (once I have everything working), you could use this container using syntax like this:

```yaml
jobs:
  <name-of-job>:
    runs-on: <name-of-runner>
    container:
      image: ghcr.io/ijlyttle/pyqr-base:latest
```

The container(s) in this repo run as `root` because the quarto and r-lib actions include some `sudo` commands. 
A couple of points that I can't quite reconcile:

- It's considered bad form to put into production a container that runs as `root`; to this point, the Rocker containers each implement a `docker` user.
- On github.com, the publicly-available runners run as `root`; the quarto and r-lib actions depend on this to install stuff.

## Acknowledgements

This work has a bunch of inspirational sources, which include:

- Brian Holt's [Complete Introduction to Containers (feat. Docker)](https://frontendmasters.com/courses/complete-intro-containers/), offered by Frontend Masters.
- Danielle Navarro's blog post, [*Playing with Docker*](https://blog.djnavarro.net/posts/2023-01-01_playing-with-docker/), and its [accompanying repository](https://github.com/djnavarro/arch-r).
- The [rocker-org/ubuntu-lts](https://github.com/rocker-org/ubuntu-lts) set of Docker images, maintained by Dirk Eddelbuettel et al.
