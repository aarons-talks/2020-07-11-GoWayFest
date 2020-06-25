# Building An App with Go Modules and Athens :tada:

Hey Gophers! We're gonna build an awesome webapp with [Gin](https://github.com/gin-gonic/gin). Well, it's pretty basic but it shows cat and dog pictures so it's still pretty awesome :grinning:.

The cool part though? Instead of pulling all my webapp's dependencies directly from version control systems like GitHub (which we've always been doing in the past), we're gonna build it using module proxies and [Athens](https://docs.gomods.io).

In fact, we're actually gonna do the build in two different ways! We'll talk about each as we go. I developed and tested this demo on a Zsh shell on MacOS.

>I gave this demo at [GoWayFest 2020](https://goway.io/) in the session titled "How to Safely and Efficiently Build your App with Modules".

![athens banner](./athens-banner.png)

# About the Web App

The web application we're going to build is fairly simple, but not trivial. I built a little server using [gin](https://github.com/gin-gonic/gin) as the framework, and it shows some HTML pages with cat pictures on them.

The cool thing is that the whole codebase is modules-aware. That means it has a `go.mod` file that keeps track of all my code's dependencies. The built-in [Go Modules](https://github.com/golang/go/wiki/Modules) system reads that file to look up the dependencies it needs to download before it builds my server.

# Run The Demo

This demo is very simple to run because all the tools we'll be using are inside of the `go` CLI.

>You'll need to have Go 1.13 installed to run this demo.

`proxy.golang.org` is the module server that the CLI uses by default (as of 1.13). That means that we won't be pulling code from version control anymore!

In this new world, _versioned_ code is stored in a global cache on disk, so we'll need to delete that:

```console
$ sudo rm -rf $(go env GOPATH)/pkg/mod
```

>Module code at a specific version should be immutable. For example, if somebody changes version X on GitHub, the actual code that `proxy.golang.org` serves won't change. The `go` tool respects that rule and stores code in this directory under very strict permissions, so that it can't be accidentally overwritten. That's why we need to `sudo rm -rf` it.

Next, we'll run the server with a simple `go run .`. With modules and Go version 1.13, `go run` will download all dependencies from `proxy.golang.org`, store them in the cache, compile the binary with all of the module code, and then run the binary.

```console
$ go run .
```

You'll see some output that looks like this:

```console
go: downloading github.com/gobuffalo/plush v3.8.3+incompatible
go: downloading github.com/gin-gonic/gin v1.4.0
go: downloading github.com/pkg/errors v0.8.1
go: downloading github.com/gobuffalo/helpers v0.4.0
go: downloading github.com/gobuffalo/github_flavored_markdown v1.1.0
go: downloading github.com/mattn/go-isatty v0.0.7
go: downloading github.com/gin-contrib/sse v0.0.0-20190301062529-5545eab6dad3
go: downloading github.com/golang/protobuf v1.3.1
go: downloading golang.org/x/net v0.0.0-20190620200207-3b0461eec859
go: downloading github.com/ugorji/go v1.1.4
<...>
```

This is the go tool downloading dependencies from `proxy.golang.org`. After the downloading steps, it extracts the code (it's downloaded as zip files) and then verifies the checksums with the `go.sum` file.

After the server runs, you'll be able to see it on [localhost:8081](http://localhost:8081)

# Keep on rockin', Gophers!

![athens gopher](./athens-gopher.png)
