---
index: 4
title: "Assets"
---

The assets package provides a way to manage web assets in your web application. It receives a folder where your assets are on the disk and returns an http.Handler that serves these files.

This handler is capable of a few things that are useful for web development:
- Asset fingerprinting (to avoid caching issues)
- Hot code reloading (when the assets change on disk)

## Usage

To set your assets in the app, you need to use the `server.WithAssets` server option. It receives an `embed.FS` parameter that will help locate template files to be served.


```go
//go:embed templates/**/*.html
var templatesFS embed.FS

r := server.New(
	server.WithAssets(templatesFS, "/files"),
)
```

## Fingerprinting Helper

The `server.WithAssets` option also sets up the `assetPath` helper within the request `context`, which can be utilized in your templates to access the fingerprinted version of an asset.

```html
<link rel="stylesheet" href="<%= assetPath(`css/app.css`) %>">
// will output something like
<link rel="stylesheet" href="/files/css/app-cafe123ff22112eedd.css">
```

### Alternative setup

Depending on your project configuration, you can also set the asset manager manually and use its fingerprint function

```go
var (
	//go:embed *
	files embed.FS

	// assets manager
	manager = assets.NewManager(files, "/files")
)

func AssetPath(name string) (string, error) {
	return manager.PathFor(name)
}

// ...
r := server.New()
r.Folder(manager.HandlerPattern(), manager)
```
