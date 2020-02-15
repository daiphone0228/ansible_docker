FROM amazonlinux:2

# TimeZone
RUN cp /usr/share/zoneinfo/Japan /etc/localtime

# update yum
RUN yum update -y \
    && yum upgrade -y

# install basement
RUN yum install -y \
        curl \
        git \
        libxml2 \
        libxml2-devel \
        httpd \
        mysql \
        gcc \
        vim \
        make \
        mod_ssl \
        wget \
        unzip \
        tar

# psコマンド等を叩けるようにする
RUN yum install -y yum-utils


CMD ["/bin/bash"]
