# Run

## Run migrations

```bash
~/laravel-scaffold$ php artisan migrate
```


## Run seeders

This seeder fill lists to database

```bash
~/laravel-scaffold$ php artisan db:seed
```


## Run Faker Seeder _optional_

This seeder create 10 users to database

```bash
~/laravel-scaffold$ php artisan db:seed --class=FakerTableSeeder
```

!> If exist someting error run this command


```bash
~/laravel-scaffold$ composer dumpautoload
```

!> If you are in production environment change storage permmision

```bash
~/laravel-scaffold$ sudo chown -R www-data:www-data storage
```

## Compiling Assets (Laravel Mix)

```bash
// Watching Assets For Changes
~/laravel-scaffold$ npm run watch

// Run all Mix tasks...
~/laravel-scaffold$ npm run dev

// Run all Mix tasks and minify output...
~/laravel-scaffold$ npm run production
```


## Run serve

Local Development Server

If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the serve Artisan command. This command will start a development server at [http://localhost:8000](http://localhost:8000)

```bash
~/laravel-scaffold$ php artisan serve
```
