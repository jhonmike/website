# Go Lang

Check that Go is installed correctly by building a simple program, as follows:

```go
hello.go

package main

import "fmt"

func main() {
    fmt.Println("Hello, Arch!")
}
```

Then run it with the go tool:

```shell
$ go run hello.go

Hello, Arch!
```

Compilation with standard gc compiler (same as go build -compiler=gc hello.go):

```shell
$ go build hello.go
$ ./hello

Hello, Arch!
```


# Simple Api GO

```go
package main

import (
    "fmt"
    "html"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
    })

    log.Fatal(http.ListenAndServe(":8080", nil))
}
```
