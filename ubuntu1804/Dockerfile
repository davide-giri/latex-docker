FROM ubuntu:18.04

# create non-root user
RUN useradd -ms /bin/bash latexuser
# switch to non-root user
USER latexuser
WORKDIR /home/latexuser
USER root

# copy over bashrc script
COPY bashrc /root/.bashrc
COPY bashrc /home/latexuser/.bashrc

# copy over tmux.conf script
COPY tmux.conf /root/.tmux.conf
COPY tmux.conf /home/latexuser/.tmux.conf

# copy over emacs script
COPY emacs /root/.emacs
COPY emacs /home/latexuser/.emacs

# set location to avoid interactive questions by apt install
ENV TZ=Europe/London
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# install various packages
RUN apt update
RUN apt install -y \
python python-pip python3 python3-pip \
xterm csh ksh zsh tmux git tig wget unzip emacs vim x11-apps gnuplot evince \
texlive-full texlive-generic-extra texlive-fonts-extra \
r-base r-base-dev libcurl4-openssl-dev libssl-dev libxml2-dev

# install tidyverse (which contains ggplot2)
COPY ggplot2.R .
RUN /usr/bin/Rscript ggplot2.R

# enable X11 forwarding
RUN sed -i 's/#   ForwardX11 no/ForwardX11 yes/g' /etc/ssh/ssh_config
