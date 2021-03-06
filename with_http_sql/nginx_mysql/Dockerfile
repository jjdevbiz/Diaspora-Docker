FROM chocobozzz/diaspora-docker:without_http_sql
MAINTAINER Florian Bigard <florian.bigard@gmail.com>

ENV MYSQLTMPROOT temprootpass

# Update and upgrade
RUN apt-get update
RUN apt-get -y upgrade

# Install Apache
RUN apt-get -y install -t wheezy-backports nginx
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# Install MySQL
RUN echo mysql-server mysql-server/root_password password $MYSQLTMPROOT | debconf-set-selections;\
  echo mysql-server mysql-server/root_password_again password $MYSQLTMPROOT | debconf-set-selections;\
    apt-get install -y mysql-server

RUN apt-get clean

ADD install.sh /home/diaspora/install.sh
ADD install_diaspora.sh /home/diaspora/install_diaspora.sh
ADD start.sh /home/diaspora/start.sh

ONBUILD ADD database.yml  /home/diaspora/diaspora/config/database.yml
ONBUILD ADD diaspora.yml  /home/diaspora/diaspora/config/diaspora.yml
ONBUILD ADD diaspora.crt  /home/diaspora/diaspora.crt
ONBUILD ADD diaspora.key  /home/diaspora/diaspora.key
ONBUILD RUN /home/diaspora/install.sh

ONBUILD ADD diaspora.conf /etc/nginx/sites-enabled/diaspora.conf
ONBUILD RUN rm /etc/nginx/sites-enabled/default

ONBUILD EXPOSE 80
ONBUILD EXPOSE 443
ONBUILD CMD ["/bin/bash", "/home/diaspora/start.sh"] 
