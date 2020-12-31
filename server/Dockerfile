FROM ubuntu:18.04

RUN apt update && \
	apt upgrade -y

RUN apt update && \
	apt install -y coreutils findutils bash openssh-server wget curl iproute2 tmux netcat-openbsd net-tools inetutils-ping tree sudo build-essential make cmake git openbsd-inetd

RUN curl https://getmic.ro | bash && \
	mv micro /usr/local/bin

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
RUN dpkg-reconfigure --frontend=noninteractive locales
RUN update-locale LANG=en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN TZ=Asia/Ho_Chi_Minh && \
	ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
	echo $TZ > /etc/timezone

RUN apt update && \
	apt install -y zip unzip mysql-client software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt update
RUN apt upgrade -y
RUN apt update && \
	apt install -y nginx php7.4-fpm php7.4-common php7.4-mysql php7.4-xml php7.4-curl php7.4-gd php7.4-imagick \
					php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip \
					php-xdebug

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
	chmod +x wp-cli.phar && \
	mv wp-cli.phar /usr/local/bin/wp

COPY appctl /usr/local/bin/appctl
COPY response/ /app

RUN [ -f /app/nginx.conf ] && mv /app/nginx.conf /etc/nginx/sites-enabled/default
RUN echo "fastcgi_param HTTP_HOST \$http_cf_worker if_not_empty;" >> /etc/nginx/fastcgi.conf
RUN echo "fastcgi_param HTTP_X_FORWARDED_HOST \$http_cf_worker if_not_empty;" >> /etc/nginx/fastcgi.conf
RUN echo "fastcgi_param HTTPS on;" >> /etc/nginx/fastcgi.conf

WORKDIR /app

ENTRYPOINT [ "appctl" ]
CMD [ "run" ]
