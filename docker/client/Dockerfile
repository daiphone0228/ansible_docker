FROM amazonlinux:2

# TimeZone
RUN cp /usr/share/zoneinfo/Japan /etc/localtime

# update yum
RUN yum update -y \
    && yum upgrade -y

# install basement
RUN yum -y install \
        sudo \
        shadow-utils \
        procps \
        wget \
        openssh-server \
        openssh-clients \
        which \
        iproute \
        e2fsprogs \
        yum-utils
# RUN yum install -y \
#         curl \
#         git \
#         libxml2 \
#         libxml2-devel \
#         httpd \
#         mysql \
#         gcc \
#         vim \
#         make \
#         mod_ssl \
#         wget \
#         unzip \
#         tar

RUN yum clean all

RUN wget https://bootstrap.pypa.io/ez_setup.py -O - | sudo python

RUN systemctl enable sshd.service

RUN echo "PasswordAuthentication no" >> /etc/ssh/sshd_config

# RUN mkdir -p /etc/ansible \
#     && echo 'localhost' > /etc/ansible/hosts \
#     && echo -e """\
# \n\
# Host *\n\
#     StrictHostKeyChecking no\n\
#     UserKnownHostsFile=/dev/null\n\
# """ >> /etc/ssh/ssh_config


RUN useradd ec2-user

RUN echo "ec2-user:ec2-user" | chpasswd

RUN echo "ec2-user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN sudo -u ec2-user mkdir -p /home/ec2-user/.ssh

COPY ./conf/ansible.pub /home/ec2-user/.ssh/authorized_keys

# RUN mv /tmp/ec2-user.pem /home/ec2-user/.ssh/ && \
RUN chmod -R go-rwx /home/ec2-user/.ssh && \
    # 公開鍵を登録
    # cat /home/ec2-user/.ssh/ec2-user.pem >> /home/ec2-user/.ssh/authorized_keys && \
    chown -R ec2-user:ec2-user /home/ec2-user/.ssh 

# ロケールを設定
RUN echo "export LANG=en_US.UTF-8" >> /home/ec2-user/.bash_profile

CMD ["/sbin/init"]
# CMD ["/bin/bash"]
