---
title: Getting Started
index: 2
---

The only requirement to get started with Leapkit is to have Go installed on your machine. You can download Go from [here](https://golang.org/dl/). Make sure you install the latest version of Go.
Once you have installed Go, you can proceed to generate your first projects

```sh
go run go.leapkit.com/tools/new@latest [project_name]
```

### Setup
Once the project is generated you can go into the project folder and run the following command:

```sh
go mod download
```

This will install the necessary dependencies.

### Running the application
Running the application will depend on the version of Go you have intalled for Go 1.23 you can run:

```sh
go run go.leapkit.com/tools/dev@latest
```

If you're using Go 1.24 you can set that as a tool in your `go.mod` file:

```sh
# This will be needed once.
go get -tool go.leapkit.com/tools/dev

# Then every time you want to run the application
go tool dev
```

This will read the Procfile in the root of the folder and start the processes defined in it. Once the application is running it should be [accessible locally](http://localhost:3000).
