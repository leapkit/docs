---
title: Environment Variables
index: 5
---

Leapkit provides a handy way to load environment variables from a `.env` file. This feature is useful when you want to define environment variables in a file instead of setting them directly in your system—especially in `development` mode.

## Default `.env` Configuration

The Leapkit template includes a `.env` file with default values that you can use as a starting point:

```.env
DATABASE_URL="leapkit.db"
GO_ENV="development"
PORT="3000"
SESSION_SECRET="secret"
SESSION_NAME="leapkit_session"
```

### Understanding Environment Variables

`GO_ENV`: Specifies the application's environment mode. When set to `development`:

- The `assets` core package recalculates fingerprinted paths for assets whenever they are updated.
- The `dev` tool listens to the `.go` file (or the extension you specify via command flag) for changes and automatically reloads the application.
- The built-in `logger` middleware prints error stack traces.

`SESSION_SECRET` and `SESSION_NAME`: These env variables are used by the `session` core package to manage sessions in your app. If your app doesn’t require session management, you can remove these variables. [Read more](/core/session.html).

`HOST` and `PORT`:  These variables define the application's host and port. By default, Leapkit sets them to `0.0.0.0` and `3000`, respectively, but you can customize them as you need.

### Adding Comments

You can also add comments to your `.env` file by starting the line with a `#` character.

```.env
# This is a comment
PORT=8080
```
### Multi-line values

You can also add multi-line values to your `.env` file by wrapping the value between quotes .

```.env
GITHUB_SECRET_KEY="-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAqTmwQppL07nBl/0TEQ5sHcqj/Iz9BmuaaEu26jMXYt1QttHn
-----END RSA PRIVATE KEY-----"
```

## Loading the environment variables

To load the environment variables from the `.env` file into your application, perform an underscore import in your `main.go` file:

```go
import (
    // Load environment variables
    _ "go.leapkit.dev/core/tools/envload"
)
```

