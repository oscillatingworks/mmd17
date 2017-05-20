Introduction to Go + HTTP
=========================

Resources for the Go + HTTP workshop taking place in the [Murcia Meetup Day 2017](http://www.murciameetupday.es/)
on 20th May 2017 by [Fernando Álvarez](https://twitter.com/fern4lvarez).

![](https://upload.wikimedia.org/wikipedia/commons/2/23/Golang.png)

Requirements
------------

* Internet connection
* Terminal: bash, zsh, etc.
* Browser
* Text editor: vim, SublimeText, Atom, Visual Code, Emacs, etc.

Getting Started
---------------

Go is an open source programming language that makes it easy to build simple, reliable, and efficient software.
Created at Google in 2009, Go is compiled, statically typed, garbage collected, memory safe and concurrent language.

Main resources:
* https://golang.org
* https://github.com/golang/go
* https://twitter.com/golang
* https://golang.org/doc/
* http://www.golangbootcamp.com

Installation
------------

* Downloads: https://golang.org/dl/
  * Linux: https://golang.org/doc/install#tarball
  * Mac: https://golang.org/doc/install#osx
  * Windows: https://golang.org/doc/install#windows
* `mkdir $HOME/go`
  * This will be the home of your Go projects, also called `GOPATH`.
  * Protip: set `GOPATH` anyway.
    * `export GOPATH=$(go env GOPATH)`
* `export PATH=$PATH:$(go env GOPATH)/bin`
  * Add this to your `.bashrc` or similar to export your Go binaries.

Hello World
-----------

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, Murcia!")
}
```

```bash
mkdir $GOPATH/src/github.com/USER/hello-murcia
cd $GOPATH/src/github.com/USER/hello-murcia
$EDITOR main.go
go build
./hello-murcia
```

Building Projects
-----------------

### `go run`

 * Executes a file on the flywithout installing it.
 * Similar to `ruby`, `python` or `bash`.
 * `go run main.go`

### `go get`
  * Downloads the source code, compiles it and installs the binary in `$GOPATH/bin`.
  * Examples:
    * `go get`
    * `go get github.com/fern4lvarez/piladb/...`
    * `go get -u github.com/o1egl/govatar`

### `go install`
  * Compiles the source code and installs the binary in `$GOPATH/bin`

### `go build`
  * Compiles the source code and installs the binary as an output file.
  * By default, in current dir.
  * Cross compile: produce binaries for foreign platforms.
  * Examples:
    * `go build`
    * `go build -o my_binary`
    * `GOOS=darwin GOARCH=386 go build`

Tooling
-------

### `go test`

* Run your tests.

### `go list`

* List your project packages.

### `gofmt`

* Automatically format your code.
* https://blog.golang.org/go-fmt-your-code

### `goimports`

* Like `gofmt`, but manage import of packages automatically.

### `go vet`

* Reports potential errors that otherwise compile.

### `golint`

* Prints out style mistakes.
* `go get -u github.com/golang/lint/golint`

### And more!

* https://github.com/alecthomas/gometalinter#supported-linters

Variables
---------

```go
package main

import "fmt"

const hello = "Hello"

var name = "Bob"
var age int

func main() {
	var city = "Murcia"

	fmt.Printf("%s, %s!\n", hello, city)
	fmt.Printf("Your name is %s and you are %d years old\n", name, age)

	// country := "Spain"
	// age = 30
	// name = "Fernando"
	// fmt.Printf("%s, %s!\n", hello, country)
	// fmt.Printf("Your name is %s and you are %d years old\n", name, age)
}
```

Command Line Parameters
-----------------------

```go
package main

import (
	"flag"
	"fmt"
)

func main() {
	var city = flag.String("city", "Murcia", "Name of the city") // *int
	var age = flag.Int("age", 0, "An age in number")          // *string
	flag.Parse()

	fmt.Printf("Hello, %s!\n", *city)
	fmt.Printf("You are %d years old\n", *age)
}
```

```bash
go build
./hello-murcia -city Berlin -age 30
```

Array, Slices, Maps
-------------------

```go
package main

import "fmt"

func main() {
	// Array
	var a [5]int
	a[4] = 100
	fmt.Println(a)

	// Slice
	s := make([]string, 5)
	s[2] = "Hello"
	fmt.Println(s)
	s = append(s, "Murcia", "Bob")
	fmt.Println(s)

	s2 := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
	fmt.Println(s2[1:4])

	// Maps
	m := make(map[string]int)
	m["key"] = 10
	fmt.Println(m["key"])
}
```

Control Structures
------------------

```go
package main

import "fmt"

func main() {
	// initialize i to 0; check each time that i is less than 5; increment i for each loop after the first
	for i := 0; i < 5; i++ {
		fmt.Println("Value of i is now:", i)
	}

	// prints increasing value of i infinitely, since the middle check statement does not exist
	//for i := 0; ; i++ {
	//	fmt.Println("Value of i is now:", i)
	//}

	// prints "value of i is: 0" infinitely since the incrementing part is not there and i can never reach 3
	// for i := 0; i < 3; {
	//	fmt.Println("Value of i:", i)
	// }

	// here both initialization and incrementing is missing in the for statement, but it is managed outside of it
	s := ""
	for s != "aaaaa" {
		fmt.Println("Value of s:", s)
		s = s + "a"
	}

	//since there are no checks, this is an infinite loop
	//i := 0
	//for {
	//	fmt.Println("Value of i is:", i)
	//	i++
	//}

	// iterate an Slice
	cities := []string{"Madrid", "Lisbon", "Paris", "Roma", "Berlin"}
	for i, name := range cities {
		fmt.Printf("City Number %d is %s\n", i, name)
	}

	for _, name := range cities {
		fmt.Printf("City is %s\n", name)
	}

	for i := range cities {
		fmt.Printf("City Number %d\n", i)
	}

	// iterate a Map
	m := map[string]string{
		"Madrid": "Spain",
		"Lisbon": "Portugal",
	}

	for city, country := range m {
		fmt.Printf("%s is the capital of %s\n", city, country)
	}

	for city := range m {
		fmt.Printf("%s is the capital of %s\n", city, m[city])
	}

	// check existence of key in Map
	realCountry, ok := m["Lisbon"]
	fmt.Printf("It's %v that %s exists\n", ok, realCountry)

	_, ok = m["Roma"]
	fmt.Printf("It's %v that %s exists\n", ok, "Italy")
}
```

Concurrency
-----------

### Goroutines

A goroutine is a lightweight thread managed by the Go runtime.

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(1 * time.Second)
		fmt.Println(s)
	}
}

func main() {
	go say("world")
	say("hello")
}
```

### Channels

Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`.

```go
package main

import "fmt"

func sum(a []int, c chan int) {
	sum := 0
	for _, v := range a {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	a := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(a[:len(a)/2], c)
	go sum(a[len(a)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

HTTP Server
-----------

```go
package main

import (
	"io"
	"net/http"
)

func hello(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "Hello world!")
}

func main() {
	http.HandleFunc("/", hello)
	http.ListenAndServe(":8000", nil)
}
```

HTTP Client
-----------

```go
package main

import (
	"fmt"
	"io"
	"log"
	"net/http"
	"os"
)

func main() {
	if len(os.Args) != 2 {
		fmt.Fprintf(os.Stderr, "Usage: %s URL\n", os.Args[0])
		os.Exit(1)
	}
	response, err := http.Get(os.Args[1])
	if err != nil {
		log.Fatal(err)
	}

	defer response.Body.Close()
	if _, err = io.Copy(os.Stdout, response.Body); err != nil {
		log.Fatal(err)
	}
}
```

Exercise
--------

### httpfs

Manage files and dirs from a HTTP server: https://github.com/oscillatingworks/httpfs

### Install

```bash
go get -u github.com/oscillatingworks/httpfs
```

### Challenge #1

* Add command line flag to run HTTP server in custom port.
* By default it runs in `8080`.

### Challenge #2

* Implement `POST /$SOME_PATH type=dir` => `mkdir -p $HOME/SOME_PATH`
* You need to extend `HandlePost` handler when the type is a directory.

### Challenge #3

* Implement writing on a file:
  * `PUT /$FILE_PATH content=text` => `echo "text" >> $HOME/FILE_PATH`
* All the logic will be implemented in `HandlePut`.

### Challenger #4

* Implement deleting files and directories:
  * `DELETE /$FILE_PATH` => `rm $HOME/$FILE_PATH`
  * `DELETE /$DIR_PATH` => `rm -rf $HOME/DIR_PATH`
* You need to create a new handler for `DELETE` actions.
* Handler with care!
