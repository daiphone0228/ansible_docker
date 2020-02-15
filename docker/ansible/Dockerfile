FROM alpine:3.10.4

# Metadata入れるかはちょっと考える

ARG ENV_ANSIBLE_PORT
ARG ENV_ANSIBLE_CLIENT_PORT
ARG ENV_ANSIBLE_VERSION
ARG ENV_ANSIBLE_LINT_VERSION

RUN apk --update --no-cache add \
        ca-certificates \
        git \
        openssh-client \
        openssl \
        python3\
        rsync \
        sshpass

RUN apk --update add --virtual \
        .build-deps \
        python3-dev \
        libffi-dev \
        openssl-dev \
        build-base \
    && pip3 install --upgrade \
        pip \
        cffi \
    && pip3 install \
        ansible==${ENV_ANSIBLE_VERSION} \
        ansible-lint==${ENV_ANSIBLE_LINT_VERSION} \
    && apk del \
        .build-deps \
    && rm -rf /var/cache/apk/*

RUN mkdir -p /etc/ansible \
    && echo 'localhost' > /etc/ansible/hosts \
    && echo -e """\
\n\
Host *\n\
    StrictHostKeyChecking no\n\
    UserKnownHostsFile=/dev/null\n\
""" >> /etc/ssh/ssh_config


# SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# EXPOSE 80
# COPY entrypoint.sh /etc/entrypoint.sh
# RUN chmod +x /etc/entrypoint.sh
CMD ["/bin/sh"]
# CMD [ "ansible-playbook", "--version" ]

