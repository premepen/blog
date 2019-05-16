FROM debian:buster

ENV WORKDIR=/opt/

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
cat > /etc/apt/sources.list <<- EOF
deb http://mirrors.ustc.edu.cn/debian buster main contrib non-free
deb-src http://mirrors.ustc.edu.cn/debian buster main contrib non-free
deb http://mirrors.ustc.edu.cn/debian buster-proposed-updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/debian buster-proposed-updates main contrib non-free
deb http://mirrors.ustc.edu.cn/debian buster-updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/debian buster-updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/debian sid main contrib non-free
EOF

RUN apt-get update -qq && apt-get install -y gcc make git git-core \
  curl build-essential openssl libssl-dev

# 安装php
RUN apt-get install -y php:7.2

# 安装 composer
RUN cd /usr/local/src/ && php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
php composer-setup.php && php -r "unlink('composer-setup.php');" && ln -sn /usr/local/src/composer.phar /usr/bin/composer

## 修复常见的composer版本不兼容错误
RUN composer install --ignore-platform-reqs

# 安装 node 和 npm; 如果出错可以尝试编译安装, 也是可以的
RUN apt-get update -qq && apt-get install -y nodejs npm


RUN cd ${WORKDIR} && git clone https://github.com/premepen/blog && cd ${WORKDIR}/blog \
&& composer install -vvv && npm install && npm run dev && php artisan blog:install

## 2019-5-15 错误 ventor 错误


