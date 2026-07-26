FROM python:3.14@sha256:5f1cdbcab9a50594a79502dd73e885456d2a2fc31f1a1fa18484815b37ee9152
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
