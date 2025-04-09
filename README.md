# tendermint-rs

[![Crate][crate-image]][crate-link]
[![Docs][docs-image]][docs-link]
[![Build Status][build-image]][build-link]
[![Audit Status][audit-image]][audit-link]
[![Apache 2.0 Licensed][license-image]][license-link]
![Rust Stable][rustc-image]

[Tendermint] in Rust with [TLA+ specifications](/docs/spec).

Tendermint is a high-performance blockchain consensus engine for Byzantine fault
tolerant applications written in any programming language.

## Tendermint Core Compatibility

tendermint-rs has been tested for compatibility with Tendermint Core v0.34.21.

## Requirements

Tested against the latest stable version of Rust. May work with older versions.

### Semantic Versioning

We do our best to follow [Semantic Versioning](https://semver.org/). However, as
we are pre-v1.0.0, we use the MINOR version to refer to breaking changes and the
PATCH version for features, improvements, and fixes.

We use the same version for all crates and release them collectively.

## Documentation

```mermaid
graph TD
    %% Level 1: External System
    EC["External Tendermint\nCore (Go)"]:::external

    %% Level 2: RPC & P2P
    subgraph "RPC & P2P"
        R["rpc"]:::net
        P2P["p2p"]:::net
    end

    %% Level 3: Light Client Components
    subgraph "Light Client Components"
        LC["light-client"]:::lib
        LCV["light-client-verifier"]:::lib
        LCD["light-client-detector"]:::lib
        LCLI["light-client-cli"]:::lib
        LCJS["light-client-js"]:::lib
    end

    %% Level 4: Core Libraries
    subgraph "Core Libraries"
        T["tendermint"]:::lib
        PROTO["proto"]:::lib
    end

    %% Level 5: ABCI Application Framework
    subgraph "ABCI Application Framework"
        A["abci"]:::lib
    end

    %% Level 6: Support & Tools
    subgraph "Support & Tools"
        CFG["config"]:::tool
        TST["test"]:::tool
        TOLS["tools"]:::tool
        DOCS["docs"]:::doc
    end

    %% Connections between External system and RPC layer
    EC -->|"RPC_request"| R
    R -->|"RPC_response"| EC

    %% RPC & P2P feed into Light Client components
    R -->|"provides_headers"| LC
    P2P -->|"feeds_network"| LC

    %% Internal Light Client interactions
    LC -->|"verifies"| LCV
    LC -->|"detects"| LCD
    LC -->|"CLI_wrapper"| LCLI
    LC -->|"JS_interface"| LCJS

    %% Core Library usage across components
    LC -->|"uses"| T
    LC -->|"serialization"| PROTO
    R -->|"serialization"| PROTO
    A -->|"interfaces"| T
    A ---|"utilizes"| T

    %% Support & Tools connecting to core components
    TOLS -.-> LC
    TST -.-> LC
    TST -.-> R
    CFG -.-> R
    DOCS -.-> T
    DOCS -.-> A

    %% Click Event Mapping
    click T "https://github.com/informalsystems/tendermint-rs/tree/main/tendermint/"
    click PROTO "https://github.com/informalsystems/tendermint-rs/tree/main/proto/"
    click A "https://github.com/informalsystems/tendermint-rs/tree/main/abci/"
    click LC "https://github.com/informalsystems/tendermint-rs/tree/main/light-client/"
    click LCD "https://github.com/informalsystems/tendermint-rs/tree/main/light-client-detector/"
    click LCLI "https://github.com/informalsystems/tendermint-rs/tree/main/light-client-cli/"
    click LCJS "https://github.com/informalsystems/tendermint-rs/tree/main/light-client-js/"
    click LCV "https://github.com/informalsystems/tendermint-rs/tree/main/light-client-verifier/"
    click R "https://github.com/informalsystems/tendermint-rs/tree/main/rpc/"
    click P2P "https://github.com/informalsystems/tendermint-rs/tree/main/p2p/"
    click CFG "https://github.com/informalsystems/tendermint-rs/tree/main/config/"
    click TST "https://github.com/informalsystems/tendermint-rs/tree/main/test/"
    click TOLS "https://github.com/informalsystems/tendermint-rs/tree/main/tools/"
    click DOCS "https://github.com/informalsystems/tendermint-rs/tree/main/docs/"

    %% Styles Definitions (dark+light mode friendly)
    classDef lib fill:#1f77b4,color:#fff,stroke:#0d3d61,stroke-width:2px;
    classDef net fill:#ff7f0e,color:#000,stroke:#8a4b00,stroke-width:2px;
    classDef tool fill:#bcbd22,color:#000,stroke:#808000,stroke-width:2px;
    classDef doc fill:#2ca02c,color:#fff,stroke:#165016,stroke-width:2px;
    classDef external fill:#d62728,color:#fff,stroke:#8c1c13,stroke-width:2px;
```

See each component for the relevant documentation.

Libraries:

- [tendermint](./tendermint) - Tendermint data structures and serialization
- [tendermint-abci](./abci) - A lightweight, low-level framework for building
  Tendermint ABCI applications in Rust
- [tendermint-light-client](./light-client) - Tendermint light client library
  for verifying signed headers and tracking validator set changes
- [tendermint-light-client-detector](./light-client-detector) - Library for
  detecting and reporting attacks against the Tendermint light client
- [tendermint-light-client-cli](./light-client-cli) - CLI for the light client,
  for verifying headers, detecting attacks and reporting them.
- [tendermint-light-client-js](./light-client-js) - Low-level WASM interface for
  interacting with the Tendermint light client verification functionality
- [tendermint-p2p](./p2p) - At present this primarily provides the ability to
  connect to Tendermint nodes via Tendermint's [secret connection](tendermint-secret-conn)
- [tendermint-proto](./proto) - Protobuf data structures (generated using Prost)
  for wire-level interaction with Tendermint
- [tendermint-rpc](./rpc) - Tendermint RPC client and response types

## Releases

Release tags can be found on
[GitHub](https://github.com/informalsystems/tendermint-rs/releases).

Crates are released on [crates.io](https://crates.io).

## Contributing

The Tendermint protocols are specified in English in the [tendermint/tendermint
repo](https://github.com/tendermint/tendermint/tree/main/spec). Any protocol
changes or clarifications should be contributed there.

This repo contains the TLA+ specifications and Rust implementations for various
components of Tendermint. See the [CONTRIBUTING.md][contributing] to start
contributing.


## Resources

Software, Specs, and Documentation

- [Tendermint Datastructures Spec](https://github.com/tendermint/spec)
- [Tendermint in Go](https://github.com/tendermint/tendermint)
- [Docs for Tendermint in Go](http://docs.tendermint.com/)

Papers

- [The latest gossip on BFT consensus](https://arxiv.org/abs/1807.04938)
- [Ethan Buchman's Master's Thesis on Tendermint](https://atrium.lib.uoguelph.ca/xmlui/handle/10214/9769)

## License

Copyright © 2020 Informal Systems and contributors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use the files in this repository except in compliance with the License.
You may obtain a copy of the License at

    https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[//]: # (badges)

[crate-image]: https://img.shields.io/crates/v/tendermint.svg
[crate-link]: https://crates.io/crates/tendermint
[docs-image]: https://docs.rs/tendermint/badge.svg
[docs-link]: https://docs.rs/tendermint/
[build-image]: https://github.com/informalsystems/tendermint-rs/workflows/Rust/badge.svg
[build-link]: https://github.com/informalsystems/tendermint-rs/actions?query=workflow%3ARust
[audit-image]: https://github.com/informalsystems/tendermint-rs/workflows/Audit-Check/badge.svg
[audit-link]: https://github.com/informalsystems/tendermint-rs/actions?query=workflow%3AAudit-Check
[license-image]: https://img.shields.io/badge/license-Apache2.0-blue.svg
[license-link]: https://github.com/interchainio/tendermint-rs/blob/master/LICENSE
[rustc-image]: https://img.shields.io/badge/rustc-stable-blue.svg

[//]: # (general links)

[tendermint-docs-link]: https://docs.rs/tendermint/
[tendermint-rpc-docs-link]: https://docs.rs/tendermint-rpc/
[Tendermint]: https://github.com/tendermint/tendermint
[tendermint-light-client-docs-link]: https://docs.rs/tendermint-light-client/
[tendermint-secret-conn]: https://github.com/tendermint/tendermint/blob/v0.34.x/spec/p2p/peer.md#authenticated-encryption-handshake
[contributing]: ./CONTRIBUTING.md
