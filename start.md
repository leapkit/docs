---
title: Getting Started
index: 2
---

The only requirement to start using Leapkit is to have installed **Go 1.24 or higher** on your machine. You can download Go from [here](https://golang.org/dl/). Ensure that you install the latest version of Go.

Once you have installed Go, you can proceed to generate your first project:

```sh
go run rsc.io/tmp/gonew@latest go.leapkit.dev/template@latest [name]
```

This command will create a new directory with the name of your app into the current path.

## Folder structure

The Leapkit directory will contain the following structure:

```text
├── bin/
├── cmd/
│    ├── app/
│    │    └── main.go
│    ├── migrate/
│    │    └── migrate.go
├── internal/
│    ├── home/
│    │    ├── index.go
│    │    └── index.html
│    ├── migrations/
│    │    ├── 0_migrations.sql
│    │    ├── 20241113163842_add_pragmas.sql
│    │    └── migrations.go
│    ├── system/
│    │    ├── assets/
│    │    │    ├── application.js
│    │    │    ├── assets.go
│    │    │    └── tailwind.css
│    │    └── layout.html
│    └── app.go
├── .dockerignore
├── .env
├── .gitignore
├── database.db
├── Dockerfile
├── go.mod
├── go.sum
├── Procfile
└── README.md
```

Navigate into the project folder and download the project dependencies:

```sh
go mod download && go tool tailo download
```
_The `go tool tailo download` command is required to fetch the Tailwind CSS binary._

### Starting the Application

To run the application, you can use the `go tool dev` command. [Read more](/tools/serve.html).

```sh
go tool dev
```

Once the application runs, it should be accessible locally at [https://localhost:3000](https://localhost:3000).
