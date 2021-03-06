FROM ubuntu:20.04
MAINTAINER Aaron Wei.

ENV PYTHONUNBUFFERED 1
ENV DEBIAN_FRONTEND=noninteractive

EXPOSE 8000

# Setup directory structure
RUN mkdir /dubber
WORKDIR /dubberRoot/dubber/

COPY dubber/requirements/base.txt /dubberRoot/base.txt
COPY dubber/requirements/dev.txt /dubberRoot/requirements.txt

# install Python 3
RUN apt-get update && apt-get install -y \
        software-properties-common
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get update && apt-get install -y \
    python3.7 \
    python3-pip
RUN python3.7 -m pip install pip
RUN apt-get update && apt-get install -y \
    python3-distutils \
    python3-setuptools

# install Postgresql
RUN apt-get -y install curl ca-certificates gnupg
RUN curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
RUN apt-get update
RUN apt-get -y install postgresql-client-11


# Install some dep
RUN apt-get install net-tools
RUN apt-get install -y libpq-dev python-dev

RUN pip3 install -r /dubberRoot/requirements.txt

# Copy application
COPY ./dubber/ /dubberRoot/dubber/

# Cronjob
RUN apt-get -y install systemd
RUN apt-get -y install cron

RUN apt-get -y install memcached
RUN apt-get update && apt-get -y install vim