FROM python:3.14@sha256:311ea5bb79f1a238ee9e38f8d5f09cb3b4b244575cf49e27cf365ea7e60f11d4
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
