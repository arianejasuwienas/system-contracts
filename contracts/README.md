**A library for hedera secure smart contract development.**

## Overview

### Installation

#### Hardhat (npm)

```
$ npm install @hashgraph/contracts
```

### Prerequisites

For this script to work, you need to communicate with the Hedera consensus node precompiles.
To access them, you must first start a local Hedera node.

1. Start a Solo Node

    Make sure you have the following prerequisites set up:
    
    * **Node.js** ≥ 20.19.0
      * **Docker** and **Docker Compose** installed and running
    
    Then start a local Hedera Solo node with a single command:
    
    ```bash
    npx @hashgraph/solo one-shot single deploy
    ```
    
    This launches a single-node Hedera network locally for quick testing and development.

2. Configure Hardhat to communicate with your local node

Install the required dependencies:
```bash
npm install --save-dev hardhat@^2
```

Add a `hardhat.config.ts` (or `.js`) with a `solo` network:

```ts
import { HardhatUserConfig } from "hardhat/config";

const PRIVATE_KEY = process.env.PRIVATE_KEY || "0xYOUR_ECDSA_SECP256K1_PRIVATE_KEY";

const config: HardhatUserConfig = {
  solidity: "0.8.24",
  networks: {
    solo: {
      url: "http://127.0.0.1:7546", // Solo JSON-RPC
      chainId: 298,                 // Solo chain id
      accounts: [PRIVATE_KEY],      // After starting the Solo node, you’ll be shown pre-created account IDs
    },
  },
};

export default config;
```


### Usage

Once installed, you can use the contracts in the library by importing them:

```solidity
pragma solidity ^0.8.20;

import {IHederaTokenService} from "@hashgraph/contracts/token-service/IHederaTokenService.sol";

contract MyContract {
    function calLPrecompile(address tokenAddress) external view {
        return IHederaTokenService(0x167).isToken(tokenAddress);
    }
}
```

If you're new to hedera smart contract development, head to [Developing Hedera Smart Contracts](https://github.com/hashgraph/hedera-docs/blob/main/core-concepts/smart-contracts/understanding-hederas-evm-differences-and-compatibility/README.md).

## Learn More

The Hedera network utilizes system contracts at a reserved contract address on the EVM to surface HAPI service functionality through EVM processed transactions.
These system contracts are precompiled smart contracts whose function selectors are mapped to defined network logic.
In this way EVM users can utilize exposed HAPI features natively in their smart contracts.

The system contract functions are defined in this library and implemented by the [Hedera Services](https://github.com/hashgraph/hedera-services) repo as part of consensus node functionality.

## Security

Please do not file a public ticket mentioning the vulnerability. To report a vulnerability, please send an email to <security@hashgraph.com>.

## Contribute

Contributions are welcome. Please see the [contributing guide](https://github.com/hashgraph/.github/blob/main/CONTRIBUTING.md) to see how you can get involved.

## Code of Conduct

This project is governed by the
[Contributor Covenant Code of Conduct](https://github.com/hashgraph/.github/blob/main/CODE_OF_CONDUCT.md). By
participating, you are expected to uphold this code of conduct. Please report unacceptable behavior
to [oss@hedera.com](mailto:oss@hedera.com).

## License

[Apache License 2.0](https://github.com/hashgraph/hedera-smart-contracts/blob/main/LICENSE)
