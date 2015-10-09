FROM ubuntu:14.04

ENV http_proxy 'http://172.16.0.200:800'
ENV https_proxy 'http://172.16.0.200:800'
ENV HTTP_PROXY 'http://172.16.0.200:800'
ENV HTTPS_PROXY 'http://172.16.0.200:800'

MAINTAINER Jose Irigon <irigon@gmail.com>

RUN apt-get update && apt-get install -y \
	openssh-server \
	git

RUN git config --global core.editor vim

# Add local user
RUN useradd -ms /bin/bash git

# Prepare ssh and inject root ssh key 
RUN mkdir /var/run/sshd
RUN mkdir /root/.ssh
COPY admin_pubkey/adm_ssh.pub /root/.ssh/authorized_keys

# Avoid perl complain
RUN sed -i 's/^AcceptEnv LANG LC_\*$//g' /etc/ssh/sshd_config

# Install gitolite
USER git
WORKDIR /home/git
ENV USER git
	
RUN git clone git://github.com/sitaramc/gitolite.git /home/git/gitolite
RUN cd /home/git/gitolite; git checkout 24171564e63d4072b2eeeb3e4dad7d2b231b31ec
RUN mkdir /home/git/bin
ENV PATH /home/git/bin:$PATH
RUN cd /home/git; gitolite/install -ln /home/git/bin;

COPY admin_pubkey/jose.pub /home/git/jose.pub
RUN cd /home/git; gitolite setup -pk jose.pub

USER root
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

ENV http_proxy  ''
ENV https_proxy ''
ENV HTTP_PROXY  ''
ENV HTTPS_PROXY ''
