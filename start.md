---
title: Getting Started
index: 2
---

The only requirement to get started with Leapkit is to have Go installed on your machine. You can download Go from [here](https://golang.org/dl/). Make sure you install the latest version of Go.
Once you have installed Go, you can proceed to generate your first projects

```sh
go run go.leapkit.com/tools/new@latest [project_name]
```

Once the project is generated go into the project folder and download the dependencies with:

```sh
go mod download
```
This will install the necessary dependencies.

### Starting the application
Running the application will depend on the version of Go you have intalled for Go 1.23 you can run:

```sh
go run go.leapkit.com/tools/dev@latest
```

If you're using Go 1.24 set that as a tool in your `go.mod` file. First download the tool (only once).

```sh
go get -tool go.leapkit.com/tools/dev
```

Then every time you want to run the application you can use the `go tool` shortcut to invoke the dev tool.

```sh
go tool dev
```

The dev tool reads the Procfile in the root of the folder and start the processes defined in it. Once the application is running it should be [accessible locally](http://localhost:3000).
