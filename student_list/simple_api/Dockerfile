

FROM python:3.8-buster 

LABEL maintainer="Salma CHENNOUFI <salmachennoufi99@gmail.com>"

COPY requirements.txt /requirements.txt

RUN apt update -y && apt install python3-dev libsasl2-dev libldap2-dev libssl-dev -y
RUN pip3 install -r /requirements.txt

VOLUME [ "/data" ]

COPY student_age.py /student_age.py
COPY student_age.json /data/student_age.json

EXPOSE 5000

CMD ["python3", "./student_age.py"]
