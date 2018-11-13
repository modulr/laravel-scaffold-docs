# Add Styles

> Add styles easily, in this example add us the Events module styles.

## Create Sass File

To create a [sass](http://sass-lang.com/) file, use the `touch` command:

```bash
~/modulr-laravel$ touch resources/assets/sass/modules/_events.scss
```


## Sass File Structure

```scss
// resources/assets/sass/modules/_events.scss

.events {
    .table {
        > tbody > tr:first-child > td {
            border-top-color: transparent;
        }

        .media {
            .media-left {
                padding-right: 20px;
            }
            .media-body {
                .media-heading {
                    font-size: 18px;
                }
                .event-info {
                    margin-top: 10px;
                    text-align: right;
                }
            }
        }
    }
}
```


## Register Sass File

```scss
// resources/assets/sass/app.scss

// Modules
...
@import 'modules/events';
```
