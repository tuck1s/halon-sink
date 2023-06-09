# Halon configuration template (container)

## Getting started

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Install [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension
3. Set the `HALON_REPO_USER` and `HALON_REPO_PASS` environment variables or adjust the build arguments in `.devcontainer/devcontainer.json`
4. [Reopen this folder in the container](https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container)

## Useful information

* This docker container is not configured to be production-ready and should only be used during development
* By default port 25 will be forwarded from the container to the local machine, click on the *PORTS* tab to see which randomized destination port you should use to connect to it
* If you need to restart `smtpd` after building a new startup configuration (`smtpd.yaml`) you can run `supervisorctl restart smtpd`
* If you need to reload `smtpd` after building a new running configuration (`smtpd-app.yaml`) you can run `halonctl config reload`
* If you need to see the text logs you can run `supervisorctl tail -f smtpd stderr`
