# Create Module

> Create your fist module easily, in this example create us the Events module.


## Backend

### Create Migration

To create a [migration](https://laravel.com/docs/5.5/migrations), use the `make:migration` [Artisan command](https://laravel.com/docs/5.5/artisan):

```bash
~/modulr-laravel$ php artisan make:migration create_events_table
```

Migration Structure

```php
// database/migrations/create_events_table.php

...
public function up()
{
    Schema::create('events', function (Blueprint $table) {
        $table->increments('id');
        $table->string('title');
        $table->text('description')->nullable();
        $table->date('date')->nullable();
        $table->text('place')->nullable();
        $table->time('start_time')->nullable();
        $table->time('end_time')->nullable();
        $table->json('attendees');
        $table->integer('user_id')->unsigned();
        $table->foreign('user_id')->references('id')->on('users');
        $table->timestamps();
        $table->softDeletes();
    });
}

public function down()
{
    Schema::dropIfExists('events');
}
```

[Running Migrations](https://laravel.com/docs/5.5/migrations#running-migrations)

To run all of your outstanding migrations, execute the `migrate` Artisan command:

```bash
~/modulr-laravel$ php artisan migrate
```


### Create Factory _optional_

When testing, you may need to insert a few records into your database, for example, let's create 3 events per user and attach a relationship to each user:

To create a [factory](https://laravel.com/docs/5.5/database-testing#writing-factories), use the `touch` command:

```bash
~/modulr-laravel$ touch database/factories/EventFactory.php
```

Factory Structure

```php
// database/factories/EventsFactory.php

$factory->define(App\Models\Events\Event::class, function (Faker\Generator $faker) {

    return [
        'title' => $faker->text($maxNbChars = 50),
        'description' => $faker->text,
        'place' => $faker->text(70),
        'date' => $faker->date($format = 'Y-m-d', $max = 'now'),
        'start_time' => $faker->time($format = 'H:i:s', $max = 'now'),
        'end_time' => $faker->time($format = 'H:i:s', $max = 'now'),
        'attendee' => [],
    ];

});
```

> [Faker Generator documentation](https://github.com/fzaninotto/Faker)

Using Factory to Faker Seeder

```php
// database/seeds/FakerDataFactory.php

factory(App\User::class, 10)
            ->create()
            ->each(function ($u) {
                ...
                $u->events()->saveMany(factory(App\Models\Events\Event::class, 3)->make());
            });
```

Running Seeder

```bash
~/modulr-laravel$ php artisan db:seed --class=FakerDataSeeder
```

### Create Model

The easiest way to create a [model](https://laravel.com/docs/5.5/eloquent#defining-models) instance is using the `make:model` Artisan command:

```bash
~/modulr-laravel$ php artisan make:model Models/Events/Event
```

Model Structure

```php
// app/Models/Events/Event.php

use Illuminate\Database\Eloquent\SoftDeletes;
use Illuminate\Support\Facades\Auth;

class Event extends Model
{
    use SoftDeletes;

    protected $dates = ['deleted_at'];

    protected $guarded = ['id'];

    public function user()
    {
      return $this->belongsTo(\App\User::class);
    }
}
```

Add Relation to User Model

```php
// app/User.php

class User extends Authenticatable
{
    ...
    public function events()
    {
        return $this->hasMany(\App\Models\Events\Event::class);
    }
}
```


### Create Routes

To add [routes](https://laravel.com/docs/5.5/routing)

```php
// routes/web.php

Route::middleware('auth')->group(function () {
    ...
    Route::group(['namespace' => 'Events'], function() {
        Route::get('/events', 'EventController@index');
        Route::group(['prefix' => 'event'], function() {
            Route::get('/all', 'EventController@all');
            Route::post('/store', 'EventController@store');
            Route::put('/update/{id}', 'EventController@update');
            Route::delete('/destroy/{id}', 'EventController@destroy');
        });
    });
});
```


> [Blade documentation](https://laravel.com/docs/5.5/blade)

### Create Controller

To create a [controller](https://laravel.com/docs/5.5/controllers), execute the `make:controller` Artisan command:

```bash
~/modulr-laravel$ php artisan make:controller Events/EventController
```

Controller Structure

```php
// app/Http/Controllesr/Events/EventController.php

...
use App\Models\Events\Event;

class EventController extends Controller
{
    public function index() {}
    public function all() {}
    public function store(Request $request) {}
    public function update(Request $request, $id) {}
    public function destroy($id) {}
}
```


### Create View

To create a [view](https://laravel.com/docs/5.5/views), use the `mkdir` and `touch` commands:

```bash
~/modulr-laravel$ mkdir resources/views/events && touch resources/views/events/events.blade.php
```

View Structure

```php
// resources/views/events/events.blade.php

@extends('layouts.app')

@section('content')
    <events></events> // Events Vue component
@endsection
```

## Frontend


### Create Vue Component

To create a [vue](https://vuejs.org/v2/guide/) component, use the `mkdir` and `touch` commands:

```bash
~/modulr-laravel$ mkdir resources/assets/js/components/events && touch resources/assets/js/components/events/Events.vue
```

Vue Component Structure

```js
// resources/assets/js/components/events/Events.vue

<template>
    <div class="events">
        <!-- Header -->
        <div class="container-fluid header"></div>
        <!-- Container -->
        <div class="container-fluid">
            <!-- List -->
            <div class="panel panel-default"></div>
            <!-- Init Message -->
            <div class="init-message"></div>
        </div>
        <!-- Modal New Event -->
        <div class="modal right fade" id="modalNewEvent"></div>
        <!-- Modal Edit Event -->
        <div class="modal right fade" data-backdrop="false" id="modalEditEvent"></div>
    </div>
</template>

<script>
    data() {
        return {
            error: {},
            events: [],
            newEvent: {},
            Editevent: {},
        }
    },
    mounted() {
        this.getEvents();
    },
    methods: {
        getEvents: function () {},
        storeEvent: function (e) {},
        editEvent: function (item, index) {},
        updateEvent: function (e) {},
        destroyEvent: function () {},
    }
</script>
```


### Register Vue Component

```js
// resources/assets/js/app.js

Vue.component('events', require('./components/events/Events.vue'));
```


### Add Vue Component into View

```php
// resources/views/events/events.blade.php

@extends('layouts.app')

@section('content')
    <events></events>
@endsection
```


### Add Module to Navbar

Add module easily, in this example add us the Events module in Navbar.

```js
// resources/assets/js/layout/Navbar.vue

<!-- Menu -->
<li class="dropdown menu">
    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">
        <i class="fa fa-th fa-lg" aria-hidden="true"></i>
    </a>
    <ul class="dropdown-menu dropdown-menu-right list-inline">
        ...
        <li :class="{'active': activeLink == 'events'}">
            <a href="/events">
                <i class="mdi mdi-event mdi-3x"></i>
                <br>
                <span>Events</span>
            </a>
        </li>
    </ul>
</li>
```


?> Now you are ready to [Create CRUD](createcrud.md) start [here](createcrud.md).
