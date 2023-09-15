#基础镜像
FROM python:3.9
ENV USER=dev
ENV PASSWD=dev
ENV WORKDIR=projects

# essential tools
RUN apt-get update \
    && apt-get install -y ssh openssh-server build-essential \
    gcc g++\
    # net-tools
    net-tools tar rsync \
    # python3 python3-pip \
    \wget tmux vim\
    sudo git\
    && apt-get clean

# add user and config pwd
RUN useradd -m ${USER} && yes ${PASSWD} | passwd ${USER}

# grand for pwd-less sudo
RUN echo ${USER}' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN chmod 644 /etc/sudoers

CMD ["/usr/sbin/sshd", "-D"]

USER ${USER}
RUN mkdir -p /home/${USER}/${WORKDIR}/
ENV LC_ALL C.UTF-8

# config all the requirements need in the container
COPY ./requirements.txt /home/${USER}/${WORKDIR}/
WORKDIR /home/${USER}/${WORKDIR}/
RUN pip install --no-cache-dir -r requirements.txt