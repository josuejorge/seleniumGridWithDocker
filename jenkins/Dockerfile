FROM jenkins/jenkins:lts

USER root

# Docker CLI needed so the Docker Pipeline plugin can talk to the host's
# Docker daemon (docker.sock is bind-mounted in) and spin up per-stage agents.
RUN apt-get update \
    && apt-get install -y --no-install-recommends docker-cli \
    && rm -rf /var/lib/apt/lists/*

USER jenkins
