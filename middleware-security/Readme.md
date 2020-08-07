# Middleware and Security

In this section we will be talking about middleware and security. The topic may seem a little unrelated but implementation wise they go hand in had.

## Middleware

Middleware is a function that wraps our handler. Thats all. 

```go
func Middleware(h handler) handler
```

This simple implementation has alot of power. In go functions can be passed in to other functions as a parameter. 

Say we want to add a log to every request we are serving that prints out the URL of the request. 

We can do that by creating our own middleware like so.

```go
func Logger(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    log.Println(r.URL)
    next.ServeHTTP(w, r)
  })
}
```

and then wrapping our main router

```go
log.Fatal(http.ListenAndServe(":"+port, Logger(r)))
```

On any request made to our server it will now print out the url of request on every request.