---
index: 2
title: "Database"
---

Leapkit's `db` package streamlines database management operations. It offers you support for both [**PostgreSQL**](https://www.postgresql.org/) and [**SQLite3**](https://www.sqlite.org/) database engines.

These are the tools that you can find within the `db` package:

- **Management**: Create and delete your database.
- **Connection**: Establish and manage database connections.
- **Migrations**: Modify the database data and schema structures.

Let's explore each tool in detail.

## Management

The `db` package offers two essential functions: `Create()` and `Drop()`. These functions enable you to set up and manage your database for your project. Simply provide the database URL as a parameter to create it. These functions internally identify, based on the URL structure, which *database engine* will be used to process the action.

```go
import "go.leapkit.dev/core/db"

var databaseURL = "postgres://user:password@host:port/db_name"

// Creating the database...
if err := db.Create(databaseURL); err != nil {
    // handle the error
}

// Dropping the database...
if err := db.Drop(databaseURL); err != nil {
    // handle the error
}
```

## Connection

To establish a connection with your database in your app, use the `ConnectionFn` function, which returns the `*sqlx.DB` and an `error` if the connection cannot be established.

```go
var (
    databaseURL = "postgres://user:password@host:port/db_name"

    DB = db.ConnectionFn(databaseURL)
)

conn, err := DB()
if err != nil {
    // handle the error
}
```

### Options

By default, the `ConnectionFn` function is configured to connect to a **PostgreSQL** database engine. But if you want to connect to your **SQLite3** database, you should use the `db.WithDriver()` option. With this you can define the driver to stablish the connection with the database. The allowed driver names are:

- `postgres`
- `sqlite` or `sqlite3`

```go
databaseURL = "/path/to/your/sqlite3.db"

DB = db.ConnectionFn(databaseURL, db.WithDriver("sqlite3"))
```

### Setting up drivers

So that the `db` package can connect to your database, ensure that you import the specific driver package for the database engine you want to use.

- PostgreSQL: `github.com/lib/pq`
- SQLite3: `github.com/mattn/go-sqlite3`

### Examples

#### Connecting to a PostgreSQL database engine

```go
package main

import (
    "go.leapkit.dev/core/db"
    _ "github.com/lib/pq" // importing the PostgreSQL driver.
)

var (
    databaseURL = "postgres://user:password@host:port/db_name"

    DB = db.ConnectionFn(databaseURL)
)

func myAwesomeFunc() {
    conn, err := DB()
    if err != nil {
        // handle the error
    }
    // ...
}
```

#### Connecting to a SQLite3 database engine

```go
package main

import (
    "go.leapkit.dev/core/db"
    _ "github.com/mattn/go-sqlite3" // importing the SQLite3 driver.
)

var (
    databaseURL = "/path/to/your/sqlite3.db"

    DB = db.ConnectionFn(databaseURL, db.WithDriver("sqlite3"))
)

// ...
func myAwesomeFunc() {
    conn, err := DB()
    if err != nil {
        // handle the error
    }
    // ...
}
```
## Migrations

Within the `db` package, Leapkit offers tools for database migration operations.

### Creating migrations

To create a new migration you will need to use our `db` tool, [Read more](#).


### Running Migrations

To run your database migrations, this package provides two functions: `RunMigrationsDir` and `RunMigrations`.

#### `RunMigrationsDir`

```go
err := db.RunMigrationsDir(migrationsPath string, db *sql.DB)
if err != nil {
    // handle the error
}
```

This function receives the path to the directory where migration files are stored and the `*sql.DB` connection.

#### `RunMigrations`

```go
err := db.RunMigrations(migrationsFS embed.FS, db *sql.DB)
if err != nil {
    // handle the error
}
```

This function received the `embed.FS` instance that contains the migration files and the `*sql.DB` connection.