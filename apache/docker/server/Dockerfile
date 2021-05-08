FROM php:7.3-apache
RUN apt-get update && apt-get install --no-install-recommends --assume-yes --quiet ca-certificates \
    curl \
    git \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    openssl \
    libzip-dev \
    zip \
    libxml2-dev \
  && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-install -j$(nproc) iconv \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-enable mysqli \
	&& a2enmod rewrite \
	&& docker-php-ext-install pdo pdo_mysql \
	&& docker-php-ext-install zip \
	&& a2enmod ssl \
	&& openssl genrsa -des3 -passout pass:password13 -out server.pass.key 2048 \
	&& openssl rsa -passin pass:password13 -in server.pass.key -out server.key \
	&& rm server.pass.key \
	&& openssl req -x509 -nodes -days 365 -newkey rsa:2048  -subj "/C=US/ST=Washington/L=Spokane/O=KellyBrady/OU=IT Department/CN=beckfield-two.local" -keyout server.key -out server.crt \
	&& echo "ServerName beckfield-two.local" >> /etc/apache2/apache2.conf \
	&& service apache2 restart \
	&& mkdir /etc/apache2/ssl \
	&& mv server.key /etc/apache2/ssl/ \
	&& mv server.crt /etc/apache2/ssl/ \
	&& a2enmod headers

# Mail Functionality
RUN curl -Lsf 'https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go get github.com/mailhog/mhsendmail
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail