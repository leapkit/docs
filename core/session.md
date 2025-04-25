---
index: 8
title: "Session"
---

Leapkit provides a way to set a session in your app to persist values needed throughout the app by using the [gorilla/sessions](https://github.com/gorilla/sessions) package.

> **Note:** Leapkit only uses session-based cookies to manage session state. These cookies are temporary and are automatically deleted when the are expired. Any other type of cookies is not used, such as persistent or third-party cookies.

## Setup

To set the session in your app, you have to use the `server.WithSession` server option and pass a session secret key and the session name.

```go
s := server.New(
   server.WithSession("secret_key", "session_name"),
)
```

## Handling session values and flashes

To use the session struct within your handler, retrieve it from the context using the `session.FromCtx()` function. Then, you can manage your session values according to the `gorilla/session` package [docs](https://pkg.go.dev/github.com/gorilla/sessions). For instance:

```go
func Handler(w http.ResponseWriter, r *http.Request) {
    ss := session.FromCtx(r.Context())

    // setting a session value
    ss.Values["sessionVal"] = "value"

    // getting a session value
    myVal := ss.Values["sessionVal"].(string)

    // adding a flash message
    ss.AddFlash("welcome!")
    ss.AddFlash("peter", "username_flash")
    // {
    // 	"_flash": ["welcome!"],
    // 	"username_flash": ["peter"],
    // }

    // getting flash messages
    flashes := ss.Flashes("username_flash")
    // ["peter"]
    // ...
}
```

You can omit the `session.Save()` method **only** if you use `http.ResponseWriter` methods because the response writer is replaced by a Leapkit session implementation, which saves the current session. Otherwise, you have to use it.

## Registering custom session types

To register custom session types, you can use the `session.RegisterSessionTypes()` function. This function takes a variable number of session types as arguments and registers them with the session manager.

> **WARNING:** The [RFC 6265](https://www.rfc-editor.org/rfc/rfc6265.html#section-6) advises keeping cookie sizes as small as possible to minimize network bandwidth. So please, ensure your cookie only holds essential data.

```go
type User struct {
	Name string
	Age int
	Address Address
}

type Address struct {
	City, Country string
}

session.RegisterSessionTypes(User{}, Address{})

ss.Values["user"] = User{
		Name: "John Smith",
		Age:  30,
		Address: Address{
			City:    "New York",
			Country: "USA",
		},
	}

user := ss.Values["user"].(User)
```
