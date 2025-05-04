---
index: 3
title: "Import maps"
---

Leakpit provides a tool for managing JavaScript module imports without relying on a bundler. This tool allows you to pin external modules (from npm or CDNs), download them to your project and generate the `importmap.json`.

## Installation

To use this tool you need to download it by running the following command:

```bash
go get --tool go.leapkit.dev/tools/importmap
```

## Usage

### Pinning modules

To pin external modules, use the following command:

```bash
go tool importmap pin <module_name>[@<version>] [module_name[@<version>]]
```

This will download the specified modules and generate an `importmap.json` file. By default the JSON file is created into the `./internal/system/assets` folder and downloaded JS modules are stored into a `vendor` folder within the same path in the project.

```text
├── internal/
│    ├── system/
│    │    ├── assets/
│    │    │    ├── application.js
│    │    │    ├── importmap.json
│    │    │    └── vendor/
│    │    │    |     ├── module_1@1.0.0.js
│    │    │    |     └── module_2@1.0.0.js
```

```json
// importmap.json
{
    "imports": {
        "application": "application.js",
        "module_1": "vendor/module_1@1.0.0.js",
        "module_2": "vendor/module_2@1.0.0.js"
    }
}
```

### Unpinning modules

To unpin external modules, use the `unpin` command to remove the specified modules from the `importmap.json` file and delete the downloaded JS modules from the `vendor` folder.

```bash
go tool importmap unpin <module_name> [module_name, ...]
```


### Updating modules

To update all your external modules to their last versions, use the following command:

```bash
go tool importmap update
```

If you need to update a module to an specific version, use the `pin` command:

```bash
go tool importmap pin <module_name>[@<version>]
```

### Re-download modules

If you want to re-download packages from the `importmap.json` file, use the `pristine` command:

```bash
go tool importmap pristine
```

### Watching installed modules

If you want to watch your installed modules, use the `packages` command:

```bash
go tool importmap packages
```

```bash
[info] Pinned packages:
  application to: application.css
  module_1    to: vendor/module_1@1.0.0.js
  module_2    to: vendor/module_2@1.0.0.js
```

or if you want to see a JSON view of them, use the `json` command:

```bash
go tool importmap json
```

```bash
[info] internal/system/assets/importmap.json
{
    "imports": {
        "application": "application.js",
        "module_1": "vendor/module_1@1.0.0.js",
        "module_2": "vendor/module_2@1.0.0.js"
    }
}
```

### Outdated modules

This tool provides a way to check for outdated modules. To check for outdated modules, use the `outdated` command:

```bash
go tool importmap outdated
```

```bash
[info] outdated packages:
  module_1 pinned: 1.0.0, latest: 1.2.3
  module_2 pinned: 1.0.0, latest: 1.2.3
```

### Audit modules

To audit your modules, use the `audit` command:

```bash
go tool importmap audit
```

```bash
[info] audit report:
[info] Report results:

module_1@1.0.0
  Severity            "high"
  Description         "Vulnerability description"
  Vulnerable versions "<1.0.0"
```
