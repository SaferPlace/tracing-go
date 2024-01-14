# SaferPlace Go Tracing

Opinionated tracing package for Go.

## Usage

The package is designed for the tracing config to be loaded from a config file. However this step
is optional and you can just provide the tracing config directly

```go

import "github.com/saferplace/tracing-go"

type ConfigFile struct {
    // Tracing is now part of your config file.
    Tracing *tracing.Config `yaml:"tracing"`
}

// Parse the config however you see fit.
```

You can then use the package to the create the tracing provider.

```go

import "github.com/saferplace/tracing-go"

func main() {
    // Load the config from file or write it directly.
    cfg := &tracing.Config{
        Enabled: true,
        // Other options
    }
    tp, close, err := tracing.NewTracingProvider(context.Background(), cfg)
    if err != nil {
        log.Fatalf("unable to create tracing provider: %v", err)
    }
    defer func() {
        _ = close()
    }()

    // use the tracing provider
    tracer := tp.Tracer("example")

    // ...
}

```
