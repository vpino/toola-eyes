# Installing PostgreSQL in Debian or Canima

Before installing PostgreSQL, make sure that you have the latest information from the Debian/Canaima repositories by updating the apt package list with:

```bash
$ sudo apt update
```

There are several packages that start with postgresql:

    postgresql-9.4: The PostgreSQL server package
    postgresql-client-9.4: The client for PostgreSQL
    postgresql: A "metapackage" better explained by the Debian Handbook or the Debian New Maintainers' Guide

To install directly the postgresql-9.4 package:

```bash
$ sudo apt install postgresql-9.4 postgresql-client-9.4
```

## Accessing the PostgreSQL Database

On Debian/Canaima, PostgreSQL is installed with a default user and default database both called postgres. To connect to the database, first you need to switch to the postgres user by issuing the following command while logged in as root (this will not work with sudo access):

First login with su/root:

```
$ su
```

Second:

```
$ su - postgres
```

You now should be logged as postgres. To start the PostgreSQL console, type psql:

```
$ psql
```

## Creating New Roles

By default, Postgres uses a concept called "roles" to aid in authentication and authorization. These are, in some ways, similar to regular Unix-style accounts, but PostgreSQL does not distinguish between users and groups and instead prefers the more flexible term "role".

Upon installation PostgreSQL is set up to use "ident" authentication, meaning that it associates PostgreSQL roles with a matching Unix/Linux system account. If a PostgreSQL role exists, it can be signed in by logging into the associated Linux system account.

The installation procedure created a user account called postgres that is associated with the default Postgres role.

To create additional roles we can use the createuser command. Mind that this command should be issued as the user postgres, not inside the PostgreSQL console:

First:

```
$ su - postgres
```

Second:

```
$ createuser --interactive
```

This basically is an interactive shell script that calls the correct PostgreSQL commands to create a user to your specifications. It will ask you some questions: the name of the role, whether it should be a superuser, if the role should be able to create new databases, and if the role will be able to create new roles. The man page has more information:

```bash
$ man createuser
```

Creating a New Database

PostgreSQL is set up by default with authenticating roles that are requested by matching system accounts. (You can get more information about this at postgresql.org). It also comes with the assumption that a matching database will exist for the role to connect to. So if I have a user called ```victor```, that role will attempt to connect to a database called ```victor``` by default.

You can create the appropriate database by simply calling this command as the postgres user:

```bash
$ createdb victor
```

The new database ```victor``` now is created.

## Connecting to PostgreSQL with the New User

Let's assume that you have a Linux account named ```victor```, created a PostgreSQL ```victor``` role to match it, and created the database ```victor```. To change the user account in Linux to ```victor```:

First:

```bash
$ su - victor
``` 

Second:

```bash
$ psql
``` 

## Another way to connect to postgres

Execute:

```bash
psql -d mypgdatabase -U mypguser
```

If you want next erros:

```bash
psql: FATAL:  Ident authentication failed for user "mypguser"
```

edit pg_hba.conf in /etc/postgresql/X.Y/main/pg_hba.conf:

```bash
local   all         all                               trust     # replace ident or peer with trust
```

reload postgresql

```bash
$ /etc/init.d/postgresql reload
```


### Install graphical mode  to Postgres

Install pgadmin3

```bash
sudo apt install pgadmin3
```

### How to connect to a remote database

```bash
psql -h <host> -p <port> -u <database>
psql -h <host> -p <port> -U <username> -W <password> <database>
``` 


### Sources

[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-9-4-on-debian-8]

[https://wiki.debian.org/es/PostgreSql]

