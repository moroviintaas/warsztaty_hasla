FROM debian:bullseye
RUN apt update
RUN apt install -y python3  python3-pip python3-dev hashcat procps

RUN pip3 install jupyter cryptography
RUN mkdir /home/labuser && useradd -d /home/labuser labuser
EXPOSE 8888



COPY notebooks /home/labuser/notebooks
RUN chown labuser -R /home/labuser
USER labuser
WORKDIR /home/labuser

CMD ["--ip", "0.0.0.0", "--notebook-dir", "/home/labuser/"]
ENTRYPOINT ["/usr/local/bin/jupyter-notebook" ]
#ENTRYPOINT ["/bin/bash"]