FROM kasmweb/ubuntu-focal-dind-rootless:1.11.0-rolling
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########

RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y sudo htop iputils-ping software-properties-common zsh unzip nano
RUN wget https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Hack.zip
RUN unzip Hack.zip -d /usr/local/share/fonts

RUN echo 'kasm-user ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN usermod -aG sudo kasm-user
RUN passwd --delete kasm-user

RUN wget https://starship.rs/install.sh
RUN chmod +x install.sh
RUN ./install.sh -y

RUN echo 'eval "$(starship init bash)"' >> .bashrc
COPY config/starship.toml .config/starship.toml

COPY assets/background.jpg /usr/share/extra/backgrounds/bg_default.png

######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000
