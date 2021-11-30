FROM debian:bullseye
RUN apt update
RUN apt install -y python3  python3-pip python3-dev hashcat procps

RUN pip3 install jupyter cryptography
RUN mkdir /home/labuser && useradd -d /home/labuser labuser
EXPOSE 8888



COPY notebooks /home/labuser/notebooks
COPY hashes /home/labuser/hashes

RUN apt install -y fonts-firacode wget
RUN wget  https://starship.rs/install.sh && chmod +x install.sh && ./install.sh --yes
COPY starship.toml /home/labuser/.config/starship.toml
RUN echo 'eval "$(starship init bash)"' >> /home/labuser/.bashrc


RUN chown labuser -R /home/labuser
USER labuser
WORKDIR /home/labuser

CMD ["--ip", "0.0.0.0", "--notebook-dir", "/home/labuser/"]
ENTRYPOINT ["/usr/local/bin/jupyter-notebook" ]
#ENTRYPOINT ["/bin/bash"]