# Configure

## Generate .env

```bash
~/laravel-scaffold$ cp .env.example .env
```


## Generate APP_KEY

```bash
~/laravel-scaffold$ php artisan key:generate
```


## Generate Symbolic link to Storage

```bash
~/laravel-scaffold$ php artisan storage:link
```


## Create Data Base

```bash
~/laravel-scaffold$ mysql -u{user} -p{password}
mysql> create database laravel_scaffold
```


## Database config params

Edit .env file

```bash
~/laravel-scaffold$ nano .env
```

Into .env file fill the data base vars

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_scaffold
DB_USERNAME=user
DB_PASSWORD=password
```
