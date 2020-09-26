# Example of CORS in FastHttpRouter

Shows how CORS can be enabled and used!

### CORS example

```go
package main

import (
  "fmt"
  "log"

  "github.com/georgecookeIW/fasthttprouter"
  "github.com/valyala/fasthttp"
)

// Index is the index handler
func HelloWorld(ctx *fasthttp.RequestCtx) {
  fmt.Fprint(ctx, "Hello World!\n")
}

// Hello is the Hello handler
func Echo(ctx *fasthttp.RequestCtx) {
  fmt.Fprintf(ctx, "hello, %s!\n", ctx.UserValue("name"))
}

func main() {
  router := fasthttprouter.New()

  router.HandleOPTIONS = true
  router.HandleCORS.Handle = true
  router.HandleCORS.AllowOrigin = "*.foo.com"
  router.HandleCORS.AllowMethods = []string{"GET"}
  
  router.GET("/", HelloWorld)
  router.GET("/echo/:name", Echo)

  log.Fatal(fasthttp.ListenAndServe(":8080", router.Handler))
}

```