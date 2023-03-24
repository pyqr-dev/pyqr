# pyqr

The name "pyqr" is an amalgamation of "Python", "Quarto" and "R".

pyqr is a data-science stack built on these foundations, with the ambition to run on:

 - MacOS
 - Windows, using WSL Ubuntu
 - Ubuntu
 - Docker, using Ubuntu images

The goal is to make the development experience independent of the back end, using VS Code as the IDE. 
I would like to extend to other IDEs, e.g. RStudio and PyCharm, but I gotta start somewhere.

I know that this includes yet another set of container images, which is especially egregious given all the existing well-supported, well-established container projects. I don't know of a container project that does quite this, so I'm using this as a learning experience. 

I want to make it easier for you to combine:

- any version of Python, using [pyenv](https://github.com/pyenv/pyenv)
- any version of R, using [rig](https://github.com/r-lib/rig)
- any version of Quarto (I wish there were a Quarto installer/updater)

Further, I want to make it easier to manage a set of dotfiles that you can deploy to a pyqr instance you might create.

## Tools

This project uses Ansible to install software for "live" computers, e.g. MacOS and Ubuntu (including WSL).
It also builds equivalent Docker containers, at three levels:

- **base**: designed for GitHub Actions, can run `actions/setup-python`, etc.
- **batch**: designed for GitHub Actions, but has Python, Quarto, R already installed.
- **active**: designed for interactive use, has zsh, stow, ansible, etc. installed.

## Inspirations

- [Isabella Velásquez' blog post on setting up a Mac for R](https://ivelasq.rbind.io/blog/macos-rig/) got me wondering if the idea could be extended.

- [ThePrimegean's Developer Productivity](https://frontendmasters.com/courses/developer-productivity/) course at Frontend Masters.

- [Danielle Navarro's blog post on building Docker containers](https://blog.djnavarro.net/posts/2023-01-01_playing-with-docker/); her effort to make the topic accessible started me down this road. 
