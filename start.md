---
title: Getting Started
index: 2
---

The only requirement to get started with Leapkit is to have Go installed on your machine. You can download Go from [here](https://golang.org/dl/). Make sure you install the latest version of Go.
Once you have installed Go, you can proceed to generate your first projects

```
go run go.leapkit.com/tools/new@latest [project_name]
```

### Setup
Once the project is generated you can setup the project by running the following command:

```
go mod download
```

This will install the necessary dependencies and setup the project for development.

### Running the application

Running the application will depend on the version of Go you have intalled for Go 1.23 you can run:

```go
go run go.leapkit.com/tools/dev@latest
```

If you're using Go 1.24 you can set that as a tool in your `go.mod` file:

```go
// This will be needed once.
go get -tool go.leapkit.com/tools/dev

// Then every time you need to run the application
go tool dev
```

Once the application is running it should be [accessible locally](http://localhost:3000).
