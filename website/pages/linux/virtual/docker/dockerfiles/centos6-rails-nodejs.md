---
layout: page
title: Докерфайл для разработки rails и node.js приложений в centos 6
permalink: /linux/virtual/docker/dockerfile/
---

    $ vi Dockerfile

<br/>

    ### Dockerfile

    FROM centos:centos6
    MAINTAINER marley (www.marley.org)

    RUN yum -y update; yum clean all
    # RUN yum -y install epel-release; yum clean all

    ENV DOCKER_ROOT_PASSWORD root
    ENV RAILS_DEVELOPER_USERNAME developer
    ENV RAILS_DEVELOPER_PASSWORD developer

    ENV RUBY_VERSION 2.1.4
    ENV RAILS_VERSION 4.1.7
    # ==============================================
    ENV echo RAILS_DEVELOPER_USERNAME 'ALL=(ALL:ALL) ALL' >> /etc/sudoers
    # ==============================================

    RUN echo "root:$DOCKER_ROOT_PASSWORD" | chpasswd

    RUN yum install -y sudo which unzip tar bzip2 vim wget nc telnet screen tcpdump traceroute bind-utils lsof curl libcurl-devel openssl-devel git make gcc gcc-c++ kernel-devel

    RUN yum install -y readline-devel

    RUN yum install -y sqlite-devel mysql-devel postgresql-devel && \
    yum clean all

    # ====== NODE.JS =========================
    RUN curl -sL https://rpm.nodesource.com/setup | bash -
    RUN yum install -y nodejs npm
    # =======================================

    # ====== GIT 2.X =========================
    RUN yum install -y git tar gcc
    RUN yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel
    RUN yum install -y perl-ExtUtils-MakeMaker
    RUN mkdir -p /opt/git/2.2.1

    WORKDIR /tmp
    RUN git clone https://github.com/git/git.git
    WORKDIR /tmp/git

    RUN make prefix=/opt/git/2.2.1 all
    RUN make prefix=/opt/git/2.2.1 install

    # RUN yum remove -y git
    # =======================================

    RUN mkdir /projects

    RUN useradd $RAILS_DEVELOPER_USERNAME
    RUN echo "$RAILS_DEVELOPER_USERNAME:$RAILS_DEVELOPER_PASSWORD" | chpasswd

    RUN chown -R $RAILS_DEVELOPER_USERNAME /projects/

    # =================================================

    RUN whoami
    USER developer
    RUN whoami

    WORKDIR /home/developer

    ENV HOME /home/developer

    RUN echo $HOME

    RUN git clone git://github.com/sstephenson/rbenv.git $HOME/.rbenv
    RUN git clone git://github.com/sstephenson/ruby-build.git $HOME/.rbenv/plugins/ruby-build

    # =======================================
    # =========== RUBY ENVIRONMENT ==========
    RUN echo '### RUBY ON RAILS ###' >> $HOME/.bash_profile
    RUN echo 'umask 011' >> $HOME/.bash_profile
    RUN echo '' >> $HOME/.bash_profile

    RUN echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> $HOME/.bash_profile
    RUN echo 'export PATH="$HOME/.rbenv/shims:$PATH"' >> $HOME/.bash_profile
    RUN echo 'eval "$(rbenv init -)"' >> $HOME/.bash_profile
    RUN echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> $HOME/.bash_profile

    RUN echo '### RUBY ON RAILS END ###' >> $HOME/.bash_profile
    # =======================================

    ENV PATH $HOME/.rbenv/bin:$PATH
    ENV PATH $HOME/.rbenv/shims:$PATH

    RUN rbenv install $RUBY_VERSION
    RUN rbenv global $RUBY_VERSION

    RUN echo "gem: --no-ri --no-rdoc" > $HOME/.gemrc

    RUN gem update --system
    RUN gem update

    RUN gem install bundler --no-ri --no-rdoc
    RUN rbenv rehash

    RUN gem install rails -v $RAILS_VERSION --no-ri --no-rdoc
    RUN rbenv rehash

    # =======================================
    # =========== GIT 2.X ENVIRONMENT ==========
    RUN echo '' >> $HOME/.bash_profile
    RUN echo '' >> $HOME/.bash_profile
    RUN echo '#### GIT ##############################' >> $HOME/.bash_profile

    RUN echo 'export GIT_HOME=/opt/git/2.2.1' >> $HOME/.bash_profile
    RUN echo 'export PATH=$PATH:$GIT_HOME/bin' >> $HOME/.bash_profile

    RUN echo '#### GIT END ##########################' >> $HOME/.bash_profile
    # =======================================

    # OPEN PORT 22 FOR ENABLING SSH
    # EXPOSE 22

    # OPEN PORT 80 FOR ENABLING HTTP
    # EXPOSE 80

    RUN source ~/.bash_profile

    CMD ["/bin/bash"]




Создать image с удалением промежуточных контейнеров в случае успешного билда  

    $ docker build --rm -t centos6/rais:v01 .

    <!--

    -for-development-rails-and-nodejs-apps-on-centos/

    -->
