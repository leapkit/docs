---
index: 2
title: "Database management"
---

The go tool db create provides several commands for managing the database.

```bash
go tool db <commands>
```

## Creating a new database

To create a new database, you need to run the `go tool db create` command. This command creates a new database using the value from the `DATABASE_URL` environment variable. The type of database engine will depend on the type of database URL set. Currently, Leapkit supports the creation of `SQLite3` and `PostgreSQL` databases.

```yaml
# .env
DATABASE_URL=postgres://user:password@host:5432/database
```

```bash
go tool db create
✅ Database created successfully
```

## Deleting database

To delete the existing database, you need to run the `go tool db drop` command. This command permanently deletes the database, **so use with caution**.

```bash
go tool db drop
✅ Database dropped successfully
```

## Resetting database

The `go tool db reset` command drops the existing database, creates a new one, and runs pending migrations. This command is useful for quickly resetting the database to a clean state.

```bash
go tool db reset
✅ Database reset successfully
```

## Migrations

### Creating a new migration

To generate a new migration, you can use the `go tool db generate_migration` command.

```bash
go tool db generate_migration create_users_table
✅ Migration file `20060102150405_create_users_table.sql` generated
```

This command will create a new migration file in the `internal/migrations` directory. If you need to place the migration file in a different location, you can use the `--migration.folder` flag.

```bash
go tool db generate_migration create_users_table --migration.folder=custom/path/migrations
✅ Migration file `20060102150405_create_users_table.sql` generated
```

### Running migrations

To migrate the database to the latest version, you need to run the `go tool db migrate`. This command applies any pending migrations to the database, ensuring it is up-to-date with the latest changes.

```bash
go tool db migrate
✅ Migrations ran successfully
```

By default, the toll will run all pending migrations located in the `internal/migrations` directory. If you need to specify a different location, you can use the `--migration.folder` flag.

```bash
go tool db migrate --migration.folder=custom/path/migrations
✅ Migrations ran successfully
```
