FROM phusion/baseimage

MAINTAINER Alexandr Medvedev <alexandr.mdr@gmail.com>

ENV HOME /root

RUN apt-get -y update

RUN apt-get -y install \
        openssh-server \
        git

RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales

# To avoid annoying "perl: warning: Setting locale failed." errors,
# do not allow the client to pass custom locals, see:
# http://stackoverflow.com/a/2510548/15677
RUN sed -i 's/^AcceptEnv LANG LC_\*$//g' /etc/ssh/sshd_config

RUN mkdir -p /var/run/sshd

RUN adduser --system --group --shell /bin/sh git
RUN su git -c "mkdir /home/git/bin"

RUN cd /home/git; su git -c "git clone git://github.com/sitaramc/gitolite";
RUN cd /home/git/gitolite; su git -c "git checkout 8f1fd8481aaa338a02f5eb2f41dff4f8f1bc96f";
RUN cd /home/git; su git -c "gitolite/install -ln";

# https://github.com/docker/docker/issues/5892
RUN chown -R git:git /home/git

# http://stackoverflow.com/questions/22547939/docker-gitlab-container-ssh-git-login-error
RUN sed -i '/session    required     pam_loginuid.so/d' /etc/pam.d/sshd

ADD ./build/init.sh /init
RUN chmod +x /init
ENTRYPOINT ["/init", "/usr/sbin/sshd", "-D"]

EXPOSE 22

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
