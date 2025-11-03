# `HederaResponseCodes.sol`

> **Auto-generated library of Hedera response codes for smart contracts**

---

## Overview

`HederaResponseCodes` is an **auto-generated Solidity library** that defines all Hedera **response code constants** (as `int32`) used across the Hedera Services stack.
It provides smart contract developers and SDK tooling with **typed access** to the same response codes that the Hedera network uses internally and exposes via HAPI gRPC and mirror node APIs.

This file is generated automatically by the utility script:

```
utils/hedera-response-codes-protobuf-parser.js
```

The script parses the official [`response_code.proto`](https://github.com/hashgraph/hedera-services/blob/main/hapi/hedera-protobufs/services/response_code.proto) file from the Hedera Services repository and emits a Solidity-compatible version.

The current library reflects codes from:

```
Hedera Services version 0.59.0-SNAPSHOT
```
