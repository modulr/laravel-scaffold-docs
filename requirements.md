# Requirements

Laravel Scaffold is based on the **LAMP** stack (Linux, Apache, MySQL & PHP) and **Node**.


## Install development environment.

- [Homestead](https://laravel.com/docs/5.5/homestead)
- [Valet](https://laravel.com/docs/5.5/valet)


## Install production environment.

- [How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)

- [Como instalar LAMP (Linux, Apache, MySQL, PHP) en AWS](https://medium.com/modulr/como-instalar-lamp-stack-linux-apache-mysql-php-en-una-instancia-ec2-de-aws-con-ubuntu-16-04-bcf72b5d9937)

- [How To Install MySQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04)

!> However, if you are not using Homestead or Valet, you will need to make sure your server meets the following requirements:


### Install PHP extensions

- PHP >= 7.1.3
- OpenSSL PHP Extension - _Enabled default on php 7_
- PDO PHP Extension - _Enabled default on php 7_
- Tokenizer PHP Extension - _Enabled default on php 7_
- Mbstring PHP Extension - _Install it with_ `$ sudo apt-get install php-mbstring`
- XML PHP Extension - _Install it with_ `$ sudo apt-get install php-xml`
- Ctype PHP Extension - _Enabled default on php 7_
- JSON PHP Extension - _Enabled default on php 7_
- CURL PHP Extension - _Install it with_ `$ sudo apt-get install php-curl`
- GD PHP Extension - _Install it with_ `$ sudo apt-get install php-gd`

### Install Git

```bash
$ sudo apt-get install git
```


### Install Composer

https://getcomposer.org/download/

```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
```

Install **globally** as a system wide executable

```bash
$ sudo mv composer.phar /usr/local/bin/composer
```

!> NOTE: Composer requiere **unzip** install it with `$ sudo apt-get install unzip`


### Install Node & NPM

https://github.com/nodesource/distributions/blob/master/README.md

```bash
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
