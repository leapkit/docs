---
title: Environment Variables
index: 5
---

Leapkit provides a handy tool to load environment variables from a `.env` file. This feature is useful when you want to load your environment variables from a file instead of setting them directly in your system, like when you're in `development` mode.

The leapkit template provides a `.env` file with some default values that you can use as a starting point. This is the initial configuration:

```.env
DATABASE_URL="leapkit.db"
GO_ENV="development"
PORT="3000"
SESSION_SECRET="secret"
SESSION_NAME="leapkit_session"
```

Depending on your project, you may choose to add or remove some of these variables. For example, if your application does not use sessions, you can eliminate the `SESSION_SECRET` and `SESSION_NAME` variables.
Additionally, there is the `HOST` env variable used by Leapkit which default value is `0.0.0.0`, but you can customize it to suit your needs.

```.env
HOST="0.0.0.0"
PORT="8080"
```

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

To load the environment variables from the `.env` file into your application, perform an underscore import in your main.go file:

```go
// main.go
import _ "go.leapkit.dev/core/envload"
```

