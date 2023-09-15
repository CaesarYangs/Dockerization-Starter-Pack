#基础镜像
FROM ubuntu:18.04
ENV USER=dev
ENV PASSWD=dev
ENV WORKDIR=projects

# 安装开发环境必要的包
RUN apt-get update \
    && apt-get install -y ssh openssh-server build-essential \
    gcc g++ gdb gdbserver cmake \
    # 无需libboost可以去掉下一行
    libboost-dev \
    # net-tools 提供了ifconfig
    net-tools tar rsync \
    # 无需python3可以去掉下一行
    python3 python3-pip \
    \wget tmux vim\
    sudo git\
    && apt-get clean


#更改shell为zsh,安装on-my-zsh
RUN apt -y install zsh \
&& git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh \
&& cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc \
&& chsh -s /bin/zsh
RUN sed -i 's/robbyrussell/crunch/g' /root/.zshrc
RUN echo "source /opt/rh/devtoolset-7/enable" >> ~/.zshrc

#安装on-my-zsh插件
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting \
&& git clone https://github.com/zsh-users/zsh-autosuggestions \
${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions \
&& sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/g' /root/.zshrc

# 添加用户并配置密码
RUN useradd -m ${USER} && yes ${PASSWD} | passwd ${USER}

# 赋予sudo权限并允许无密码sudo
RUN echo ${USER}' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN chmod 644 /etc/sudoers

CMD ["/usr/sbin/sshd", "-D"]

USER ${USER}
RUN mkdir -p /home/${USER}/${WORKDIR}/
ENV LC_ALL C.UTF-8