FROM python:2

MAINTAINER WireCloud Team <wirecloud@conwet.com>

COPY ./wirecloud-dev.whl /
RUN pip install /wirecloud-dev.whl "gunicorn==19.3.0" "psycopg2==2.6"

WORKDIR /opt

RUN adduser --system --group --shell /bin/bash wirecloud; \
    wirecloud-admin startproject wirecloud_instance; \
    chown -R wirecloud:wirecloud wirecloud_instance; \
    chmod a+x wirecloud_instance/manage.py

WORKDIR /opt/wirecloud_instance

RUN sed -i "s/'ENGINE': 'django.db.backends.'/'ENGINE': 'django.db.backends.postgresql_psycopg2'/g" wirecloud_instance/settings.py; \
    sed -i "s/'NAME': ''/'NAME': 'postgres'/g" wirecloud_instance/settings.py; \
    sed -i "s/'USER': ''/'USER': 'postgres'/g" wirecloud_instance/settings.py; \
    sed -i "s/'PASSWORD': ''/'PASSWORD': 'postgres'/g" wirecloud_instance/settings.py; \
    sed -i "s/'HOST': ''/'HOST': 'postgres'/g" wirecloud_instance/settings.py; \
    sed -i "s/'PORT': ''/'PORT': '5432'/g" wirecloud_instance/settings.py; \
    sed -i "s/SECRET_KEY = '[^']\+'/SECRET_KEY = 'TOCHANGE_SECRET_KEY'/g" wirecloud_instance/settings.py; \
    sed -i "s/STATIC_ROOT = path.join(BASEDIR, '..\/static')/STATIC_ROOT = '\/var\/www\/static'/g" wirecloud_instance/settings.py

RUN python manage.py collectstatic --noinput; \
    chown -R wirecloud:wirecloud /var/www/static

# volumes must be created after running the collectstatic command
VOLUME /var/www/static
VOLUME /opt/wirecloud_instance

EXPOSE 8000

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
