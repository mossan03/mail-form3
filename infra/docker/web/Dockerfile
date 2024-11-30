FROM amazonlinux:2

ENV TZ=Asia/Tokyo \
  LANG=ja_JP.UTF-8 \
  LANGUAGE=ja_JP:ja \
  LC_ALL=ja_JP.UTF-8

RUN yum update -y \
    && yum install -y \
    glibc-langpack-ja \
    sudo \
    curl \
    gnupg \
    unzip \
    httpd \
    yum-utils \
    ca-certificates \
    && usermod -u 1000 apache \
    && groupmod -g 1000 apache

RUN amazon-linux-extras install -y epel \
    && yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm
RUN yum clean metadata\
    && yum -y install\
    php82\
    php82-php\
    php82-php-mbstring\
    php82-php-pdo\
    php82-php-gd\
    php82-php-mysqlnd\
    php82-php-mysql\
    php82-php-xml\
    php82-php-fpm
RUN alternatives --install /usr/bin/php php /usr/bin/php82 1
COPY infra/docker/web/php.ini /etc/opt/remi/php82/php.ini

RUN cd /var/www \
    && mkdir -p mail-form \
    && chown apache:apache mail-form -R \
    && chmod g+s mail-form

COPY infra/docker/web/httpd.conf /etc/httpd/conf/httpd.conf

RUN cd /usr/local/bin \
    && curl -sSL https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 -o mhsendmail \
    && chmod +x mhsendmail

CMD ["httpd", "-DFOREGROUND"]       