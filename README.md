# `rns-vpn`

> P2P VPN over Reticulum mesh network

Library and application for VPN client over Reticulum mesh network.

## Quick Start

```bash
# Clone the repository and checkout the holochain branch
git clone https://github.com/BeechatNetworkSystemsLtd/rns-vpn-rs.git
cd rns-vpn-rs
git checkout holochain

# Build the project
cargo build --release
```

## Building

Building `Reticulum-rs` requires `protoc` binary for compiling `.proto` files.

## Client configuration

`Config.toml`

`vpn_ip` -- the IP assigned to this client in CIDR format (e.g. `10.0.0.1/24`)

`peers` -- a map of `<ip> = <destination-hash>` pairs for each peer to communicate with
on the network

## Client application

Client application uses a Reticulum UDP interface that is configured with command-line
arguments.

Private keys can be generated with `openssl` tool using the `genkeys.sh` script.

Running with log level INFO will log the destination hash generated for the clients
configured private keys and should be provided to peers to add to their configurations.

Command-line options:

`-p <port>` -- required: local UDP port for Reticulum interface

`-f <ip>:<port>` -- required: IP and port for upstream Reticulum node

`[-i <name>]` -- optional: use string to generate private ID; overrides
creation of identity with `RNS_VPN_PRIVKEY_PATH`/`RNS_VPN_SIGNKEY_PATH` variables

Environment variables:

`RNS_VPN_PRIVKEY_PATH` -- path to X25519 private key in PEM format for Reticulum
identity

`RNS_VPN_SIGNKEY_PATH` -- path to ed25519 signing key in PEM format for Reticulum
identity

`RUST_LOG` -- adjust log level: `trace`, `debug`, `info` (default), `warn`, `error`

### Usage

While the client application is running and connected, peers can be reached via their
configured IP addresses and the traffic will be routed over Reticulum. For example a
peer at 10.0.0.1 serving a UDP echo server:
```
# on 10.0.0.1
$ ncat -u -l 10.0.0.1 12345 --sh-exec 'cat'
```
can now be reached via other peers:
```
# on 10.0.0.2
$ ncat -u 10.0.0.1 12345
foo
foo
```

## Documentation

This repository includes comprehensive documentation for different use cases:

- **[POC_SETUP_GUIDE.md](POC_SETUP_GUIDE.md)** - Complete proof of concept setup guide
- **[TEAM_QUESTIONS_ANSWERS.md](TEAM_QUESTIONS_ANSWERS.md)** - Answers to common setup questions
- **[HOLOCHAIN_SETUP.md](HOLOCHAIN_SETUP.md)** - Holochain integration guide
- **[TEST_RESULTS.md](TEST_RESULTS.md)** - Test results and verification

## Repository Information

- **Repository**: https://github.com/BeechatNetworkSystemsLtd/rns-vpn-rs
- **Branch**: `holochain` (contains all setup scripts and documentation)
- **Main branch**: `master` (basic VPN functionality)
