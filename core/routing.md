---
index: 1
title: "Routing"
---

Leapkit uses Go 1.22 routing as the core of its routing system. This means that you can use the same routing system as you would in a Go application. This is done by using the `http.HandlerFunc` type as the handler for the routes.

```go
r.HandleFunc("GET /{$}", channels.Default)
r.Group("/settings/", func(rg server.Router) {
	rg.Use(users.OnlyAdmin)

	rg.HandleFunc("GET /edit/{$}", settings.Edit)
	rg.HandleFunc("GET /join-token/generate/{$}", settings.GenerateJoinToken)
	rg.HandleFunc("PUT /update/{$}", settings.Update)
})
```

## LeapKit Server

The leapkit server is a wrapper around the Go `http.Server`. It provides some extra abilities to the server, such as the ability to group routes and middleware.

The `server` package is where all these features live. You can create a new server by calling `server.New`.

```go
package main

import (
	"go.leapkit.dev/core/server"
)

func main() {
	r := server.New()

	// ...

	fmt.Println("Server started at", r.Addr())
	if err := http.ListenAndServe(r.Addr(), r.Handler()); err != nil {
		fmt.Println(err)
	}
}
```

## Server options

The `server.New` function receives some options that you can use to configure the server.

```go
r := server.New(
	server.WithHost("localhost"), // Set the host of the server
	server.WithPort("8080"),      // Set the port of the server
)
```

This function returns a `server.Router` implementation so you can map your routes just like you would in a Go application.

### WithHost
WithHost allows to configure the host of the server. By default its `0.0.0.0`.

### WithPort
WithPort allows to specify the port of the server. By default its `3000`.

### WithSession
WithSession allows to set a new session into the server. [Read more](/core/session.html).

### WithAssets
WithAssets allows to set assets into the server. [Read more](/core/assets.html).

### WithErrorMessage
WithErrorMessage allows you to set your custom 404 or 500 messages. [Read more](/core/errors.html).

## Router Methods

The `server.Router` implementation has the following methods:

### Middleware (Use)

This method allows you to specify a middleware to the router.

```go
r.Use(func(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("X-Hello", "World")

		next.ServeHTTP(w, r)
	})
})
```

#### Built-in middlewares

The `server.Router` has some built-in middlewares that adds some extra functionality to your server.

- **Logging**: This middleware tracks the result of all requests.
- **Panic recovering**: Prevents the all panics from any unexpected error.
- **RequestID:** Sets the request ID into the request context.

#### The Valuer

The `valuer` is a map set in the request context that stores values for use by other components, such as the render engine. By default, this valuer stores the following information:

- `request`: Is the current *http.Request
- `currentURL`: holds the current URL from the request

To add values to the valuer, retrieve it from the context and assert it to an interface that has a `Set(string, any)` method:

```go
func(w http.ResponseWriter, r *http.Request) {
	if vlr, ok := r.Context().Value("valuer").(interface{ Set(string, any) }); ok {
		vlr.Set("api-key", "abc123")
	}
}
```

To read values from the valuer, assert it to an interface that has a `Value(string) any` method:

```go
func(w http.ResponseWriter, r *http.Request) {
	if vlr, ok := r.Context().Value("valuer").(interface{ Value(string) any }); ok {
		x := vlr.Value("api-key") // abc123
	}
}
```

### HandleFunc

This method allows you to register a new handler function for a specific pattern.

```go
r.HandleFunc("GET /{$}", func(w http.ResponseWriter, r *http.Request) {
	// ...
})
```

### Grouping Routes

The `Group` method that allows you to group routes together, this is useful to have a better organization of your routes.

```go
r.Group("/api/", func(rg *server.Router) {
	rg.HandleFunc("GET /hello/{$}", helloHandler)          // GET    /api/hello
	rg.HandleFunc("POST /hello/{$}", createHelloHandler)   // POST   /api/hello
	rg.HandleFunc("DELETE /hello/{$}", deleteHelloHandler) // DELETE /api/hello
})
```

### Folder (assets)

The `Folder` method that allows you to serve files from a folder or any other io.FS.

```go
r.Folder("/files/", templates.FS)
```
