FROM alpine:3.5

LABEL maintainers="big0rn0@club404.nowhere && johny@club404.nowhere"

# Define environnement
# ----------------------------------------------------------------------------------------------------------------------
ENV TERM=xterm-color

# Upgrade alpine with latest packages
# ----------------------------------------------------------------------------------------------------------------------
RUN apk upgrade --no-cache

# Install dev tools
# ----------------------------------------------------------------------------------------------------------------------
RUN apk add --no-cache bash git make binutils findutils grep tar less build-base \
        curl curl-dev wget ncurses grep bash-completion git-bash-completion

# Install Docker and Docker Compose
# ----------------------------------------------------------------------------------------------------------------------
RUN apk add --no-cache -X http://nl.alpinelinux.org/alpine/edge/community docker docker-bash-completion && \
    rm -rf /usr/bin/docker-runc \
           /usr/bin/docker-containerd* \
           /usr/bin/dockerd \
           /etc/conf.d/docker \
           /usr/bin/docker-proxy

# Avoid PATH variable being overrided by profile loading
# ----------------------------------------------------------------------------------------------------------------------
COPY profile /etc/profile

# Install FZF - [ https://github.com/junegunn/fzf ]
# ----------------------------------------------------------------------------------------------------------------------
RUN mkdir -p /usr/local/go && \
    apk add --no-cache go ncurses-dev && \
    export GOPATH=/usr/local/go && \
    go get -u github.com/junegunn/fzf/src/fzf && \
    cd /usr/local/go/src/github.com/junegunn/fzf && \
    cd bin && ln -s /usr/local/go/bin/fzf fzf && \
    cd .. && ./install --all && \
    apk del go ncurses-dev

# Install Bash-it - [https://github.com/Bash-it/bash-it]
# ----------------------------------------------------------------------------------------------------------------------
RUN git clone --depth=1 https://github.com/Bash-it/bash-it.git /root/.bash_it && \
    /root/.bash_it/install.sh --silent && \

    # Activate aliases, plugins and completions
    cp /root/.bash_it/aliases/available/curl.aliases.bash /root/.bash_it/aliases/enabled/. && \

    cp /root/.bash_it/plugins/available/docker.plugin.bash /root/.bash_it/plugins/enabled/. && \
    cp /root/.bash_it/aliases/available/docker.aliases.bash /root/.bash_it/aliases/enabled/. && \
    cp /root/.bash_it/completion/available/docker.completion.bash /root/.bash_it/completion/enabled/. && \

    cp /root/.bash_it/plugins/available/docker-compose.plugin.bash /root/.bash_it/plugins/enabled/. && \
    cp /root/.bash_it/aliases/available/docker-compose.aliases.bash /root/.bash_it/aliases/enabled/. && \
    cp /root/.bash_it/completion/available/docker-compose.completion.bash /root/.bash_it/completion/enabled/. && \

    cp /root/.bash_it/plugins/available/node.plugin.bash /root/.bash_it/plugins/enabled/. && \

    cp /root/.bash_it/plugins/available/git.plugin.bash /root/.bash_it/plugins/enabled/. && \
    cp /root/.bash_it/aliases/available/git.aliases.bash /root/.bash_it/aliases/enabled/. && \
    cp /root/.bash_it/completion/available/git.completion.bash /root/.bash_it/completion/enabled/. && \

    cp /root/.bash_it/plugins/available/git-subrepo.plugin.bash /root/.bash_it/plugins/enabled/. && \

    cp /root/.bash_it/plugins/available/extract.plugin.bash /root/.bash_it/plugins/enabled/. && \

    cp /root/.bash_it/plugins/available/fzf.plugin.bash /root/.bash_it/plugins/enabled/.

COPY .bashrc /root/.bashrc
COPY .aliases /root/.bash_it/aliases/custom.aliases.bash
COPY .functions /root/.bash_it/lib/custom.bash

ENTRYPOINT [ "bash" ]
