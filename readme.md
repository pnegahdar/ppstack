# ppstack: Pretty print your golang stacktrace

Simple wrapper around [panicparse](https://github.com/maruel/panicparse) to allow pretty printing at runtime for use by
panic handlers, loggers, etc.

Automatically removes itself from the stacktraces.

## Usage:

```go
import (
    "github.com/pnegahdar/ppstack"
    "os"
)


func main(){
    ppstack.Print(os.Stdout, false) // set to true to print all gorounties.
}

```

#### E.g Logging in a grpc intercepter:

```go

func unaryDebugIntercepter(enabled bool) grpc.UnaryServerInterceptor {
    return func (ctx context.Context, req interface{}, info *grpc.UnaryServerInfo, handler grpc.UnaryHandler) (interface{}, error) {
        if !enabled {
            return handler(ctx, req)
        }
        resp, err := handler(ctx, req)
        if err != nil {
            ppstack.Print(os.Stdout, false)
        }
    return resp, err
    }
}
```

