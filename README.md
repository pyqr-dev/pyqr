# pyqr

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
      image: ghcr.io/ijlyttle/pyqr-base:edge
```

At present, I'm getting things to work, hence the `edge` tag. At some point, I'll implment proper versioning.

This container runs as `root` because the quarto and r-lib actions include some `sudo` commands. 
A couple of points that I can't quite reconcile:

- It's bad form to put into production a container running as `root`. For example, the Rocker containers each implement a `docker` user.
- On github.com, the publicly-available runners run as `root`; the quarto and r-lib actions depend on this to install stuff.

## Acknowledgements

Once I started this, I came across [Posit's r-docker project](https://github.com/rstudio/r-docker), whose purpose is tantalizingly close to the purpose of this project:

  - r-docker is meant to support installation of R.
  - this project is meant to support installation of Python, Quarto, and R.

Given how close these are, and that Posit is *way* more capable than I am, we should not be surprised if this project has a short life.

This work also draws from these inspirations:

- Brian Holt's [Complete Introduction to Containers (feat. Docker)](https://frontendmasters.com/courses/complete-intro-containers/), offered by Frontend Masters.
- Danielle Navarro's blog post, [*Playing with Docker*](https://blog.djnavarro.net/posts/2023-01-01_playing-with-docker/), and its [accompanying repository](https://github.com/djnavarro/arch-r).
- The [rocker-org/ubuntu-lts](https://github.com/rocker-org/ubuntu-lts) set of Docker images, maintained by Dirk Eddelbuettel et al.
