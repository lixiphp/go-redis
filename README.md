go-redis
========

go-redis is a [Redis](http://redis.io) client library for the
[Go programming language](http://golang.org). It's built on the skeleton of
[gomemcache](http://github.com/bradfitz/gomemcache).

Licensed under the Apache License, Version 2.0.


## Status

The library is very stable and has been extensively tested on
[freegeoip.net](http://freegeoip.net) as the underlying quota mechanism
communicating with Redis. It has served dozens of billions of queries that
used this library to manage people's quota.

It is incomplete, though. Me, [@gleicon](https://github.com/gleicon) and
others have only implemented the commands we needed for our applications so
far, and continue doing so with no rush or schedule. See
[commands.go](https://github.com/fiorix/go-redis/blob/master/redis/commands.go)
for a list of supported commands - they're in alphabetical order. Contributors
are welcome.

Worth mentioning that this is not an experiment. We've written other Redis
client libraries before, also very stable and used in large deployments by
major companies.

[![Build Status](https://secure.travis-ci.org/fiorix/go-redis.png)](http://travis-ci.org/fiorix/go-redis)


## Installing

Make sure Go is installed, and both $GOROOT and $GOPATH are set, then
run:

	$ go get github.com/fiorix/go-redis/redis


## Usage

Hello world:

	import "github.com/fiorix/go-redis"

	func main() {
		rc := redis.New("10.0.0.1:6379", "10.0.0.2:6379", "10.0.0.3:6379")
		rc.Set("foo", "bar")

		v, err := rc.Get("foo")
		...
	}

When connected to multiple servers, commands such as PING, INFO and
similar are only executed on the first server. GET, SET and others are
distributed by their key.

New connections are created on demand, and stay available in the connection
pool until they time out. The library scales very well under high load.


### Unix socket, dbid and password support

The client supports ip:port or unix socket for connecting to redis.

	rc := redis.New("/tmp/redis.sock db=5 passwd=foobared")

Database ID and password can only be set by ``New()`` and can't be
changed later. If that is required, make a new connection.


## Credits

Thanks to (in no particular order):

- [Gleicon](https://github.com/gleicon) for all the drama.
- [gomemcache](https://github.com/bradfitz/gomemcache): for the skeleton of
this client library.
