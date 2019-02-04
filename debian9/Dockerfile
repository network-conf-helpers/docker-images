#
# https://github.com/network-conf-helpers/docker-images/tree/master/debian9
# https://cloud.docker.com/u/netconfhelpers/repository/docker/netconfhelpers/base
#
FROM docker.io/debian:9
MAINTAINER Denis Arnaud <denis.arnaud_github at m4x dot org>
LABEL version="0.1"

# Environment
ENV container docker
ENV HOME /root
ENV LANGUAGE en_US:en
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG

# Update the system
RUN apt-get update && apt-get -y install locales apt-utils && apt-get -y upgrade

# Generate the locales
RUN locale-gen $LANG

# Configure the time-zone
RUN apt-get -y install tzdata && echo "Europe/Paris" > /etc/timezone && \
	dpkg-reconfigure -f noninteractive tzdata

# Basic packages
RUN apt-get -y install sudo less htop net-tools wget curl screen git aptitude \
	keychain keyutils gawk bash-completion vim-nox emacs-nox \
	gzip tar lsb-release manpages
	
# Cleaning
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure SSH
RUN mkdir -p $HOME/.ssh && chmod 700 $HOME/.ssh

# Set up some convenient environment for the `root` user
ADD resources/bashrc $HOME/.bashrc
ADD resources/gitconfig $HOME/.gitconfig
ADD resources/vimrc $HOME/.vimrc
RUN chmod 640 $HOME/.bashrc $HOME/.gitconfig $HOME/.vimrc

#
WORKDIR $HOME

# Git prompt
RUN git clone https://github.com/magicmonty/bash-git-prompt.git $HOME/.bash-git-prompt --depth=1

# Entry point
CMD ["/bin/bash"]

