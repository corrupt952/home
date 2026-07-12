FROM python:3.14@sha256:09b29c360b84742bf98eba40b214f7f6b4b53286bb2c8a8b5b1afa188a8d9c0e
ENV USERNAME=ansible
RUN useradd -ms /bin/bash $USERNAME

RUN apt update \
    && apt install -y libssh-dev \
    && rm -rf /var/lib/apt/lists/*

SHELL ["/bin/bash", "-lc"]
USER $USERNAME
WORKDIR /home/$USERNAME
RUN echo "PATH=$PATH:/home/$USERNAME/.local/bin" >> /home/$USERNAME/.bashrc
COPY --chown=$USERNAME:$USERNAME requirements.txt .
RUN pip install --user -r requirements.txt
RUN ansible-galaxy collection install \
        yamaha_network.rtx \
        yamaha_network.swx \
        ansible.netcommon
