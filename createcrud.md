# Create CRUD

> Create your fist CRUD easily, in this example create us the Events module CRUD.


## Read

Get and list all events

#### Route

```php
Route::get('/all', 'EventController@all');
```

#### Controller

```php
public function all()
{
    return Event::with('user')
                  ->orderBy('id', 'desc')
                  ->paginate(20);
}
```

#### Vue Component

```js
// resources/assets/js/components/events/Events.vue

<template>
    <!-- List -->
    <div class="panel panel-default" v-if="events.length">
        <div class="panel-body">
            <table class="table table-hover">
                <tbody>
                    <tr v-for="(item, index) in events" >
                        <td>
                            <div class="media">
                                <div class="media-left">
                                    <i class="fa fa-calendar fa-4x"></i>
                                </div>
                                <div class="media-body">
                                    <h4 class="media-heading">{{item.title}}</h4>
                                    <span class="text-muted">{{item.description}}</span>
                                    <div class="event-info">
                                        <span v-show="item.place"><small class="text-muted">Place. </small>{{item.place}}</span>
                                        <br>
                                        <span v-show="item.date">{{item.date | date-format}}.</span>
                                        <span v-show="item.start_time"><small class="text-muted">From </small>{{item.start_time | time-format}}</span>
                                        <span v-show="item.end_time"><small class="text-muted">to </small>{{item.end_time | time-format}}</span>
                                    </div>
                                </div>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <!-- Init Message -->
    <div class="init-message" v-else>
        <i class="fa fa-calendar" aria-hidden="true"></i>
        <p class="lead">Don´t exist Events... create one!!</p>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                events: [],
            }
        },
        mounted() {
            this.getEvents();
        },
        methods: {
            getEvents: function () {
                axios.get('/event/all')
                .then(response => {
                    this.events = response.data.data
                });
            },
        },
    }
</script>
```

## Create

#### Route

```php
Route::post('/store', 'EventController@store');
```

#### Controller

```php
public function store(Request $request)
{
    $this->validate($request, [
        'title' => 'required|string',
    ]);

    $q = Event::create([
        'title' => $request->title,
        'description' => $request->description,
        'place' => $request->place,
        'date' => $request->date,
        'start_time' => $request->start_time,
        'end_time' => $request->end_time,
        'attendees' => '[]',
        'user_id' => Auth::id(),
    ]);

    return Event::with('user')->find($q->id);
}
```

#### Vue Component

```js
<template>
    <!-- Header -->
    <div class="container-fluid header">
        <div class="row">
            <div class="col-xs-6 header-title">
                <h1>Events</h1>
            </div>
            <div class="col-xs-6 header-buttons">
                <a href="#" class="btn btn-success"  data-toggle="modal" data-target="#modalNewEvent">
                    <i class="fa fa-calendar-plus-o"></i> New Event
                </a>
            </div>
        </div>
    </div>
    <!-- Modal New Event -->
    <div class="modal right fade" id="modalNewEvent">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title" id="myModalLabel">New Event</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group" :class="{'has-error': error.title}">
                        <label>Event *</label>
                        <input type="text" class="form-control" v-model="newEvent.title">
                        <span class="help-block" v-if="error.title">{{error.title[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.description}">
                        <label>Description</label>
                        <textarea class="form-control" rows="3" v-model="newEvent.description"></textarea>
                        <span class="help-block" v-if="error.description">{{error.description[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.place}">
                        <label>Place</label>
                        <input type="text" class="form-control" v-model="newEvent.place">
                        <span class="help-block" v-if="error.place">{{error.place[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.date}">
                        <label>Date</label>
                        <input type="date" class="form-control" v-model="newEvent.date">
                        <span class="help-block" v-if="error.date">{{error.date[0]}}</span>
                    </div>
                    <div class="row">
                        <div class="col-xs-6">
                            <label>Start Time</label>
                            <div class="form-group" :class="{'has-error': error.start_time}">
                                <input type="time" class="form-control" v-model="newEvent.start_time">
                                <span class="help-block" v-if="error.start_time">{{error.start_time[0]}}</span>
                            </div>
                        </div>
                        <div class="col-xs-6">
                            <label>End Time</label>
                            <div class="form-group" :class="{'has-error': error.end_time}">
                                <input type="time" class="form-control" v-model="newEvent.end_time">
                                <span class="help-block" v-if="error.end_time">{{error.end_time[0]}}</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default pull-left" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-success" @click="storeEvent">Create</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                error: {},
                events: [],
                newEvent: {}
            }
        },
        methods: {
            storeEvent: function (e) {
                var btn = $(e.target).button('loading')
                axios.post('/event/store', this.newEvent)
                .then(response => {
                    this.events.unshift(response.data);
                    this.newEvent = {};
                    this.error = {};
                    var btn = $(e.target).button('reset')
                    $('#modalNewEvent').modal('hide');
                })
                .catch(error => {
                    this.error = error.response.data;
                    var btn = $(e.target).button('reset')
                });
            },
        }
    }
</script>
```

## Update

#### Route

```php
Route::put('/update/{id}', 'EventController@update');
```

#### Controller

```php
public function update(Request $request, $id)
{
    $this->validate($request, [
        'title' => 'required|string',
    ]);

    $q = User::find($id);
    $q->title = $request->title;
    $q->description = $request->description;
    $q->place = $request->place;
    $q->date = $request->date;
    $q->start_time = $request->start_time;
    $q->end_time = $request->end_time;
    $q->save();

    return $q;
}
```

#### Vue Component

```js
<template>
    <!-- Modal Edit Event -->
    <div class="modal right fade" data-backdrop="false" id="modalEditEvent">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title" id="myModalLabel">Edit Event</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group" :class="{'has-error': error.title}">
                        <label>Event *</label>
                        <input type="text" class="form-control" v-model="Editevent.title">
                        <span class="help-block" v-if="error.title">{{error.title[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.description}">
                        <label>Description</label>
                        <textarea class="form-control" rows="3" v-model="Editevent.description"></textarea>
                        <span class="help-block" v-if="error.description">{{error.description[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.place}">
                        <label>Place</label>
                        <input type="text" class="form-control" v-model="Editevent.place">
                        <span class="help-block" v-if="error.place">{{error.place[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.date}">
                        <label>Date</label>
                        <input type="date" class="form-control" v-model="Editevent.date">
                        <span class="help-block" v-if="error.date">{{error.date[0]}}</span>
                    </div>
                    <div class="row">
                        <div class="col-xs-6">
                            <label>Start Time</label>
                            <div class="form-group" :class="{'has-error': error.start_time}">
                                <input type="time" class="form-control" v-model="Editevent.start_time">
                                <span class="help-block" v-if="error.start_time">{{error.start_time[0]}}</span>
                            </div>
                        </div>
                        <div class="col-xs-6">
                            <label>End Time</label>
                            <div class="form-group" :class="{'has-error': error.end_time}">
                                <input type="time" class="form-control" v-model="Editevent.end_time">
                                <span class="help-block" v-if="error.end_time">{{error.end_time[0]}}</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default  pull-left" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-link" @click="destroyEvent()">
                        <i class="fa fa-trash-o fa-lg"></i>
                    </button>
                    <button type="button" class="btn btn-primary" @click="updateEvent">Update</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                error: {},
                events: [],
                Editevent: {},
            }
        },
        methods: {
            editEvent: function (item, index) {
                this.Editevent = _.clone(item);
                this.Editevent.index = index;
                $('#modalEditEvent').modal('show')
            },
            updateEvent: function (e) {
                var btn = $(e.target).button('loading')
                axios.put('/event/update/'+this.Editevent.id, this.Editevent)
                .then(response => {
                    this.events[this.Editevent.index] = response.data;
                    this.Editevent = {};
                    this.error = {};
                    var btn = $(e.target).button('reset')
                    $('#modalEditEvent').modal('hide')
                })
                .catch(error => {
                    this.error = error.response.data;
                    var btn = $(e.target).button('reset')
                });
            }
        }
    }
</script>
```


## Delete

#### Route

```php
Route::delete('/destroy/{id}', 'EventController@destroy');
```

#### Controller

```php
public function destroy($id)
{
    return Event::destroy($id);
}
```

#### Vue Component

```js
<template>
    <!-- Modal Edit Event -->
    <div class="modal right fade" data-backdrop="false" id="modalEditEvent">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title" id="myModalLabel">Edit Event</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group" :class="{'has-error': error.title}">
                        <label>Event *</label>
                        <input type="text" class="form-control" v-model="Editevent.title">
                        <span class="help-block" v-if="error.title">{{error.title[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.description}">
                        <label>Description</label>
                        <textarea class="form-control" rows="3" v-model="Editevent.description"></textarea>
                        <span class="help-block" v-if="error.description">{{error.description[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.place}">
                        <label>Place</label>
                        <input type="text" class="form-control" v-model="Editevent.place">
                        <span class="help-block" v-if="error.place">{{error.place[0]}}</span>
                    </div>
                    <div class="form-group" :class="{'has-error': error.date}">
                        <label>Date</label>
                        <input type="date" class="form-control" v-model="Editevent.date">
                        <span class="help-block" v-if="error.date">{{error.date[0]}}</span>
                    </div>
                    <div class="row">
                        <div class="col-xs-6">
                            <label>Start Time</label>
                            <div class="form-group" :class="{'has-error': error.start_time}">
                                <input type="time" class="form-control" v-model="Editevent.start_time">
                                <span class="help-block" v-if="error.start_time">{{error.start_time[0]}}</span>
                            </div>
                        </div>
                        <div class="col-xs-6">
                            <label>End Time</label>
                            <div class="form-group" :class="{'has-error': error.end_time}">
                                <input type="time" class="form-control" v-model="Editevent.end_time">
                                <span class="help-block" v-if="error.end_time">{{error.end_time[0]}}</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default  pull-left" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-link" @click="destroyEvent()">
                        <i class="fa fa-trash-o fa-lg"></i>
                    </button>
                    <button type="button" class="btn btn-primary" @click="updateEvent">Update</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                error: {},
                events: [],
                Editevent: {},
            }
        },
        methods: {
            destroyEvent: function () {
                var self = this;
                swal({
                    title: "Are you sure?",
                    text: "You will not be able to recover this Event!",
                    type: "warning",
                    showLoaderOnConfirm: true,
                    showCancelButton: true,
                    confirmButtonText: "Yes, delete it!",
                    closeOnConfirm: false
                },
                function(){
                    axios.delete('/event/destroy/' + self.Editevent.id)
                    .then(response => {
                        self.events.splice(self.Editevent.index, 1);
                        swal({
                            title: "Deleted!",
                            text: "The Event has been deleted.",
                            type: "success",
                            timer: 1000,
                            showConfirmButton: false
                        });
                        self.error = {};
                        $('#modalEditEvent').modal('hide');
                    })
                    .catch(error => {
                        self.error = error.response.data;
                    });
                });
            },
        }
    }
</script>
```
