# What is Mongify?

[Mongify](http://mongify.com/) is a data translator system for moving your SQL data to MongoDB.

Mongify helps you move your data without worrying about the IDs or foreign IDs. It allows you to embed data into documents, including polymorphic associations. All this and more with just a few simple steps.

# How to use this image

## Create a `database.config`
This file will contain the databases configuration. Note that `host` can be a linked docker image of `mysql` or `mongodb`. The `mysql2` adapter is not an error, it's the faster version of the standard `mysql` adapter.

```ruby
sql_connection do
  adapter   "mysql2"
  host      "mysql"
  username  "root"
  password  "password"
  database  "database"
end

mongodb_connection do
  host      "mongo"
  database  "database"
end

```

Put this file where you will launch the docker image.

## Run Mongify

You can then start using Mongify running these commands:

```console
$ docker run --rm -v "$PWD":/mongify/ -it brisma/mongify mongify check database.config
$ docker run --rm -v "$PWD":/mongify/ -it brisma/mongify mongify translation database.config > translation.rb
$ docker run --rm -v "$PWD":/mongify/ -it brisma/mongify mongify process database.config translation.rb
$ docker run --rm -v "$PWD":/mongify/ -it brisma/mongify mongify sync database.config translation.rb
```

Note that `$PWD` is the current folder where `database.config` is stored.

### Using linked containers

Using linked containers is pretty simple, just add `--link` flags as always like:

```console
$ docker run --rm -it --link mysql --link mongo -v "$PWD":/mongify/ brisma/mongify mongify check database.config
```

## Run Mongify Mongoid

This image also include `mongify_mongoid`, an utility useful to generate `mongoid` models from mongify translation.
You can use mongify_mongoid running this command:

```console
$ docker run --rm -it -v "$PWD":/mongify/ brisma/mongify mongify_mongoid translation.rb
```

# Documentation

The documentation of Mongify is provided by [Anlek Consulting](http://anlek.com/) at this [address](http://mongify.com/getting_started.html).

# Supported Docker versions

This image is officially supported on Docker version 1.12.3.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see [the Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.
