FROM ubuntu:14.04

ENV http_proxy 'http://172.16.0.200:800'
ENV https_proxy 'http://172.16.0.200:800'
ENV HTTP_PROXY 'http://172.16.0.200:800'
ENV HTTPS_PROXY 'http://172.16.0.200:800'
ENV PATH=/home/git/bin:$PATH

MAINTAINER Jose Irigon <irigon@gmail.com>

RUN apt-get update && apt-get install -y \
	openssh-server \
	git 

RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales

RUN git config --global core.editor vim
	
# Prepare ssh, we can inject root ssh key to have ssh access 
# 	without break gitolite.
RUN mkdir /var/run/sshd
RUN mkdir /root/.ssh
ADD admin_pubkey/adm.pub /root/.ssh/authorized_keys

# Add local user
RUN adduser --system --group --shell /bin/bash git

# Avoid perl complain
RUN sed -i 's/^AcceptEnv LANG LC_\*$//g' /etc/ssh/sshd_config

# Install gitolite
RUN su git -c "mkdir /home/git/bin"
RUN cd /home/git; su git -c "git clone git://github.com/sitaramc/gitolite.git";
RUN cd /home/git/gitolite; su git -c "git checkout 24171564e63d4072b2eeeb3e4dad7d2b231b31ec";
RUN cd /home/git; su git -c "gitolite/install -ln";

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

ENV http_proxy  ''
ENV https_proxy ''
ENV HTTP_PROXY  ''
ENV HTTPS_PROXY ''
