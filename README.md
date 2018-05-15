## status-go-sdk
This package provides an easy way to interact with [status](https://status.im) messaging system.
This package allows you create or  application interact with [status](https://status.im) messaging system.

## Motivation
This library was initially created to have a low-dependency way to interact with status messaging system.

## Build status

[![Build Status](https://travis-ci.org/akashnimare/foco.svg?branch=master)](https://travis-ci.org/akashnimare/foco)

## Features
This project is under heavy development process, here is a list of supported features:

**Status messaging basic interaction**
- [x] Setup a new connection
- [x] Close a specific connection
- [x] Sign up on the platform
- [x] Login as a specific account
- [ ] Documented API for basic sdk interaction

**Public channels interaction**
- [x] Join public channels
- [x] Publish messages on public channels
- [x] Subscribe to public channels
- [x] Unsubscribe from a public channel
- [ ] Documented API for public channels interaction
- [x] Working examples for public channel interaction

**Contact management**
- [ ] Invite new contact
- [ ] Accept a contact request

**1 to 1 messages interaction**
- [ ] Send 1 to 1 conversation
- [ ] Subscribe to 1 to 1 conversation
- [ ] Unsubscribe from a 1 to 1 conversation
- [ ] Documented API for 1 to 1 conversation
- [ ] Working examples for 1 to 1 conversations

**Private groups interaction**
- [ ] Publish messages on private groups
- [ ] Subscribe to private groups
- [ ] Unsubscribe from a private groups
- [ ] Documented API for private groups interaction
- [ ] Working examples for private groups interaction

## Code Example
You'll find some code examples on the [examples folder](/examples)

## Installation
status-go-sdk is a lightweight dependency package, that means by now it does not have any external dependency. You can install it by running:
```
go get github.com/status-im/status-go
```

## API Reference
TBD

## Tests
TBD

## How to use?
`status-go-sdk` relies on a running instance of `statusd`, we can configure it to be accessed on many different ways, but the easiest one is like:

```
# Clone status-go
mkdir $GOPATH/src/status-im/
cd $GOPATH/src/status-im/
git clone git@github.com:status-im/status-go.git
cd status-go

# Build it and run it
make statusgo && ./build/bin/statusd -shh=true -standalone=false -http=true -status
```

With this you'll have a statusd server running on `8545` port.

`status-go-sdk` receives as parameter an [RPCClient interface](https://github.com/status-im/status-go-sdk/blob/master/sdk.go#L7), this allows you to use it in many different ways, here we will use a basic example relying on go-ethereum/rpc client.

```
package main

import (
	"fmt"
	"log"
	"time"

	"github.com/ethereum/go-ethereum/rpc"
	"github.com/status-im/status-go-sdk"
)

func main() {
  // Here we create a new remote client against our running statusd
	rpcClient, err := rpc.Dial("http://localhost:8545")
	checkErr(err)
	remoteClient := &remoteClient{rpcClient}

  // Setting up a new sdk client
	client := sdk.New(remoteClient)

  // We signup and login as a new account
	a, err := client.SignupAndLogin("password")
	checkErr(err)

  // Joining a new channel
	ch, err := a.JoinPublicChannel("my_channel")
	checkErr(err)

  // We will be sending messages to that channel every 3 seconds
	for range time.Tick(3 * time.Second) {
		message := fmt.Sprintf("PING : %d", time.Now().Unix())
		_ = ch.Publish(message)
	}
}

// This is the remote client we will use to
type remoteClient struct {
	c *rpc.Client
}

func (rc *remoteClient) Call(req *sdk.Request, res interface{}) error {
	return rc.c.Call(res, req.Method, req.Params)
}

func checkErr(err error) {
	if err != nil {
		log.Fatal(err)
	}
}
```

If you want to subscribe to this same channel you could easily do it with `Subscribe` method like:
```
ch, err := a.JoinPublicChannel("supu")
checkErr(err)

_, _ = ch.Subscribe(func(m *sdk.Msg) {
  log.Println("Message from ", m.From, " with body: ", m.Raw)

  if strings.Contains(m.Raw, "PING :") {
    time.Sleep(5 * time.Second)
    message := fmt.Sprintf("PONG : %d", time.Now().Unix())
    _ = ch.Publish(message)
  }
})

runtime.Goexit()
```

Please have a look at [examples](examples) folder for some working examples

## Contribute

Please check the [contributing guideline](CONTRIBUTING.md).

## License
[Mozilla Public License 2.0](https://github.com/status-im/status-go/blob/develop/LICENSE.md)
