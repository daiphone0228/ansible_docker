FROM alpine:3.10.4
LABEL maintainer="daiphone <daiphone0228@gmail.com>"

# Metadata入れるかはちょっと考える

ARG ENV_ANSIBLE_PORT
ARG ENV_ANSIBLE_CLIENT_PORT
ARG ENV_ANSIBLE_VERSION
ARG ENV_ANSIBLE_LINT_VERSION

RUN apk --update --no-cache add \
        ca-certificates \
        git \
        openssh-client \
        openssh-server \
        openssl \
        openrc \
        python3\
        rsync \
        sshpass

# start sshd
RUN rc-update add sshd && rc-status
RUN mkdir -p /run/openrc/ && touch /run/openrc/softlevel

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

# setting portforward
RUN sed -i 's/^AllowTcpForwarding no/AllowTcpForwarding yes/g' /etc/ssh/sshd_config
RUN sed -i 's/^GatewayPorts no/GatewayPorts yes/g' /etc/ssh/sshd_config
RUN sed -i 's/^X11Forwarding no/X11Forwarding yes/g' /etc/ssh/sshd_config

RUN mkdir -p /etc/ansible \
    && echo 'localhost' > /etc/ansible/hosts \
    && echo -e """\
\n\
Host *\n\
    StrictHostKeyChecking no\n\
    UserKnownHostsFile=/dev/null\n\
""" >> /etc/ssh/ssh_config

# create user
RUN adduser -D ansible
# It will cause an ssh login error if user has no password
RUN echo "ansible:ansible" | chpasswd
# If need sudo
RUN apk add --no-cache sudo
RUN echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# create public key
USER ansible
RUN mkdir $HOME/.ssh
RUN ssh-keygen -m PEM -t rsa -b 1024 -C "ansible@example.com" -N "" -f $HOME/.ssh/id_rsa
RUN cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys


USER root
RUN cp /home/ansible/.ssh/id_rsa $HOME/ansible.pem
# RUN echo "ansible:ansible" | chpasswd

CMD ["/sbin/init"]
# CMD ["/etc/init.d/sshd", "start"]
# CMD ["/etc/init.d/sshd", "start", "&&", "tail", "-f", "/dev/null"]

