---
title: "Learning Go (Part 1 - Getting Started)"
date: 2025-01-27T20:12:43+05:30
draft: false
description: 
tags: [blog]
---

![](https://go.dev/blog/5years/gophers5th.jpg)

## What the fuck is Go?
Go is a general perpos programming language that is staticly typed and has sooo many fucking features. Even the guys at suckless will call it bloated but there are some good reasons to use it. It's made but Robert Griesemer, Rob Pike and Ken Thompson. When you see these name you know the thing is going to be good.
## Why learn go?
IDK you search for it. If you want to you should but if you don't then it's a no
big deal. Learn something else IG.

But, jokes apart here are few reasons to learn Go:
1. **Performance**: Since it's a compiled language unlike Python or something even worse JavaScript, it runs much faster.
2. **Out of the box Concurrency Support**: Go supports concurrency with the
   help of `goroutines` and makes the code you write much better since "it's not
1970s" that our computers only have a single core. It is capable of running
multiple tasks simultaneously saving time and resources.
3. **Simplicity**: Its syntax is basically English and still manages to be so fucking fast. While some of the more advanced topics are can be intimidating, it still manages to keep the syntax as simple as it can.
4. **Good Eco-system**: The standard library has everything you need, for building programs.

Enough blabbering and selling `Go` to you. Here are some ways we can get started
writing `Go`.

## Setting up the environment
### Install Go
Run this script or just manually install it from [go.dev/doc/install](https://go.dev/doc/install).
Following script will run for Linux, IDK about BSD (should run) but apart from that I ain't supporting you guys find your own way of installing IG or just install Linux into your system.

Also be aware of what the script is doing, don't just run random scripts you find on the internet. That's how you fuck up you system.
```bash
#!/bin/sh

TMP_DIR=$(mktemp -d)
URL="https://golang.org/dl/"
TAR_FILE="go1.23.5.linux-amd64.tar.gz" # Update this to the latest version URL
INSTALL_DIR="/usr/local"

cleanup() {
    rm -rf "$TMP_DIR"
}

trap cleanup EXIT

echo "Downloading: $URL$TAR_FILE"

curl -fsSL "$URL$TAR_FILE" -o "$TMP_DIR/$TAR_FILE"
if [ $? -ne 0 ]; then
    echo "Failed to download Go."
    exit 1
fi

tar -C "$INSTALL_DIR" -xzf "$TMP_DIR/$TAR_FILE"
if [ $? -ne 0 ]; then
    echo "Error: Failed to extract Go."
    exit 1
fi

echo "Setting up Go environment"
export PATH="$INSTALL_DIR/go/bin:$PATH"

echo "Go version:"
go version
```

This should have the go version at the end, and it will be the version you mentioned in the URL.

### Now set up an editor for Go.
We will use my Neovim config from
[github.com/iamb4uc/dots](https://github.com/iamb4uc/dots) that already has all
the basic options turned on for most of the programming language use mason to
install some Go plugins (open up mason using `<leader>cm`).

Now you can write a real Go program natively in your system.
To do so, here's an example for a basic `goroutine` program.

Create a temp directory and make a project just for the time being:
```bash
$ mktemp -d 
$ cd </path/to/temp/directory> # usually its /tmp/tmp.<something>
$ touch main.go
```

Write this code, I'll explain this in another blog, but this is a basic go
program with concurrency using `goroutine`

```go
package main

import (
    "fmt"
    "time"
)

func greetings(s string) {
    for i := 0; i < 100; i++ {
        time.Sleep(time.Millisecond)
        fmt.Printf("%v: %v\n", i, s)
    }
}

func main() {
    go greetings("Hi")
    greetings("Mom")
}
```

This code is a basic `goroutine` implementation for 100 iteration.

### Last step is execution of the code 
To execute the commands we have some different tools:
1. `go run <filename>`: It runs the program just like running python. IDK its
   internal working, but it's kinda like a quick compiler that runs go code IG.
   It's good for development in my opinion.
2. `go build .`: This actually compiles the code into an executable binary
   making it way more efficient when shipping out production code.
3. `gcc-go <filename>`: Same as `go build` it's a GCC compiler instead of a
   native go compiler. But its unlike the go compiler make a lightweight
   executable with a smaller size usually.

Here is all the example for these commands:
```bash
go run main.go    # This runs the file as aspected
go build main.go  # This compiles the file into an executable e.g. main/main.exe
gccgo main.go     # This also complies the code into a less bloated executable
```

Difference between `go build` and `gccgo` when it comes to file size
```sh
$ gccgo main.go -o main
$ ll main
-rwxr-xr-x 63k iamb4uc 28 Jan 20:32 main
$ go build main.go
$ ll main
-rwxr-xr-x 2.1M iamb4uc 28 Jan 20:34 main
```

That's all for this Part from next one we will be writing some proper go code.

For the time being you can check out [**A Tour Of Go**](https://go.dev/tour/welcome/1)
