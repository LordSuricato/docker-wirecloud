FROM python:2

MAINTAINER WireCloud Team <wirecloud@conwet.com>

# Install apache2 and create a wirecloud user
RUN apt-get -y update; \
    apt-get install -y apache2 libapache2-mod-wsgi; \
    adduser --system --group --shell /bin/bash wirecloud

# Install wirecloud & dependencies
RUN pip install "wirecloud<0.9.0"

COPY ["apache-config.conf", "/etc/apache2/sites-enabled/000-default.conf"]

WORKDIR /opt

RUN wirecloud-admin startproject wirecloud_instance --quick-start; \
    chown -R wirecloud:wirecloud wirecloud_instance

WORKDIR /opt/wirecloud_instance
RUN sed -i "s/SECRET_KEY = '[^']\+'/SECRET_KEY = 'TOCHANGE_SECRET_KEY'/g" wirecloud_instance/settings.py

# The volume must be created after running the wirecloud-admin command
VOLUME /opt/wirecloud_instance

WORKDIR /opt/wirecloud_instance

EXPOSE 80

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
