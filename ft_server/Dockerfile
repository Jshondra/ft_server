# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: jshondra <marvin@42.fr>                    +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2021/01/19 14:47:40 by jshondra          #+#    #+#              #
#    Updated: 2021/01/24 15:54:58 by jshondra         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

FROM debian:buster

RUN apt -y update
RUN apt -y upgrade
RUN apt -y install wget
RUN apt -y install nginx 
RUN apt -y install mariadb-server
RUN apt -y install php-fpm php-mysql
RUN apt -y install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip

RUN apt -y install wget
RUN wget https://wordpress.org/latest.tar.gz && tar -xzvf latest.tar.gz && rm -rf latest.tar.gz
RUN mv wordpress/ /var/www/wp
COPY ./srcs/wp-config.php /var/www/wp
RUN chown -R www-data:www-data /var/www/wp
RUN chmod 660 /var/www/wp/wp-config.php

RUN wget https://files.phpmyadmin.net/phpMyAdmin/5.0.4/phpMyAdmin-5.0.4-all-languages.tar.gz && tar -xzvf phpMyAdmin-5.0.4-all-languages.tar.gz && rm -rf phpMyAdmin-5.0.4-all-languages.tar.gz
RUN mv phpMyAdmin-5.0.4-all-languages /var/www/phpmyadmin
RUN mkdir /var/www/phpmyadmin/tmp && chown www-data:www-data /var/www/phpmyadmin/tmp
RUN chmod 700 /var/www/phpmyadmin/tmp
COPY ./srcs/config.inc.php /var/www/phpmyadmin

COPY ./srcs/nginx.conf /etc/nginx/sites-available/nginx.conf
RUN ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled

COPY ./srcs/db.sh .
RUN bash db.sh && rm -rf db.sh

COPY ./srcs/ssl.sh .
RUN bash ssl.sh && rm -rf ssl.sh

COPY ./srcs/autoindex.sh .
COPY ./srcs/start.sh .

EXPOSE 80 443
CMD bash start.sh
