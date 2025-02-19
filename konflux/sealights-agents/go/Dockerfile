FROM registry.redhat.io/ubi9/go-toolset:latest

USER root

# Installs Sealights Go agent and Sealights CLI
RUN wget -qO- https://agents.sealights.co/slgoagent/latest/slgoagent-linux-amd64.tar.gz | tar -xzv -C /usr/local/bin
RUN wget -qO- https://agents.sealights.co/slcli/latest/slcli-linux-amd64.tar.gz | tar -xzv -C /usr/local/bin

USER 1001