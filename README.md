# Club 404's Development environment
Base disposable development environment is meant to provide consistency in our development tooling.
Current dev environments are:
* Default base which provides a complete bash environement with git and docker

# How to use it
1. Create [Personnal access token on Github](https://github.com/settings/tokens)

2. Create a dedicated git configuration file in your home directory wit your custom personnal configuration settings and the folloing mandatory blocks:
```
vi .gitconfig.club404
# This is Git's per-user configuration file.
[user]
        name = <YOUR_USERNAME>
        email = <YOUR_ACCOUNT_EMAIL>
[url "https://<GITHUB_USERNAME>:<GITHUB_PERSONAL_ACCESS_TOKEN>@github.com/"]
        insteadOf = https://github.com/

[alias]
        diff = diff --color-words | less -rS
[credential]
        helper = cache
```

3. Define the following alias function in your (chose the one you like) .bash_profile, .bashrc, .zshrc...
```
function club-env {
   docker run --hostname devclub404 -it --rm --init -v ${PWD}:/src -w /src -v /var/run/docker.sock:/var/run/docker.sock -v ${HOME}/.gitconfig.club404:/root/.gitconfig club404/base-env:latest
}
```

4. Then reload your shell and now you can call a dedicated dev env by calling `club-env`

# Docker Builder pattern
These images can also be used as a base builder for a component to folow docker builder pattern.
Building component in an isolated way inside a container provide a lot of benefits:
* A component defines it's build dependencies, doesn't rely on preinstalled ones on a continuous integration build agent
* Builds can be repeated
* Dependencies are managed and link to the component
* Work can be distributed
* Failures can be reproduced and even persisted as new images for troubleshooting
* Bug reports can contain a repeatable build script.

Note: Docker images are based on small [Alpine Linux image](https://hub.docker.com/_/alpine/) which provides a modern OS for a very limited *5MB* disk space usage.