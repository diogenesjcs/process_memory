FROM python:3.7.5-stretch

#
# SETUP CONTAINER
#
RUN apt-get update -y && apt-get upgrade -y && apt-get install nginx git libpython3.7-dev python3.7-dev libsnappy-dev -y

EXPOSE 80

ENV GUNICORN_TIMEOUT 120

#
# DEPLOY APP
#
RUN git clone https://github.com/onsplatform/process_memory /var/www/memory

WORKDIR /var/www/memory/

RUN pip install pipenv gunicorn

RUN pipenv install --system --deploy

#
# DEPLOY SERVICE
#
RUN cp /var/www/memory/deploy/memory.nginx /etc/nginx/sites-available/memory

RUN mkdir /var/log/memory \
 && rm -rf /etc/nginx/sites-enabled/default \
 && ln -s /etc/nginx/sites-available/memory /etc/nginx/sites-enabled/memory



#
# SETUP ENTRYPOINT
#
RUN cp /var/www/memory/deploy/docker_entrypoint.sh /docker_entrypoint.sh

RUN chmod +x /docker_entrypoint.sh

ENTRYPOINT ["sh", "/docker_entrypoint.sh"]
