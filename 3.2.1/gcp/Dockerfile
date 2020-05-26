FROM google/cloud-sdk:293.0.0-alpine

# Metadata
LABEL org.label-schema.name="helm-kubectl-secrets" \
    org.label-schema.url="https://hub.docker.com/r/cousine/helm-kubectl-secrets/" \
    org.label-schema.vcs-url="https://github.com/cousine/helm-kubectl-secrets" 

ENV HELM_VERSION="v3.2.1"

RUN gcloud components install kubectl \
    && wget -q https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -O - | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm

WORKDIR /config

RUN apk add --no-cache curl gnupg dos2unix

ENV HELM_HOME=/usr/local/helm
RUN mkdir -p /usr/local/helm/plugins \
    && helm plugin install https://github.com/zendesk/helm-secrets \
    && mkdir /root/keys

COPY docker-entrypoint.sh /usr/local/bin
RUN dos2unix /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]

ENV GPG_TTY=/dev/console
CMD bash
