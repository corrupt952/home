FROM python:3.11
ENV USERNAME=ansible
RUN useradd -ms /bin/bash $USERNAME

SHELL ["/bin/bash", "-lc"]
USER $USERNAME
WORKDIR /home/$USERNAME
RUN echo "PATH=$PATH:/home/$USERNAME/.local/bin" >> /home/$USERNAME/.bashrc
ENV ANSIBLE_VERSION=7.5.0
ENV ANSIBLE_PYLIBSSH_VERSION=1.1.0
RUN pip install \
    ansible==$ANSIBLE_VERSION \
    ansible-pylibssh==$ANSIBLE_PYLIBSSH_VERSION
RUN ansible-galaxy collection install \
        yamaha_network.rtx \
        yamaha_network.swx \
        ansible.netcommon
