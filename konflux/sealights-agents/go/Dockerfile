FROM registry.redhat.io/ubi9/go-toolset:latest

LABEL konflux.additional-tags="latest"

USER root

# Installs Sealights Go agent and Sealights CLI
RUN wget -qO- https://agents.sealights.co/slgoagent/v1.1.183/slgoagent-linux-amd64.tar.gz | tar -xzv -C /usr/local/bin
RUN wget -qO- https://agents.sealights.co/slcli/v1.0.46/slcli-linux-amd64.tar.gz | tar -xzv -C /usr/local/bin

USER 1001
