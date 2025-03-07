---
index: 2
title: "Serving your app"
---

The `dev` tool provides a way to run your Leapkit app in `development` mode. It reads the `Procfile` in the root of the folder and starts the processes defined in it.

```yaml
# Procfile
app: go run cmd/app/main.go
wrk: go run cmd/wrk/main.go
```

```bash
go tool dev
2006-01-02 03:04:05 app | Server started at 0.0.0.0:3000
2006-01-02 03:04:05 wrk | Starting worker
```

This tool also provides you with the live code reload by listening for changes in `.go` files. You can also run the `go tool dev` command with the `--watch.extensions` flag to specify the file extensions to watch for changes.

```bash
go tool dev --watch.extensions=.go,.html,.css,.js
```
