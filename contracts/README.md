**A library for hedera secure smart contract development.**

## Overview

### Installation

#### Hardhat (npm)

```
$ npm install @hashgraph/contracts
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

_If you're new to hedera smart contract development, head to [Developing Hedera Smart Contracts](https://github.com/hashgraph/hedera-docs/blob/main/core-concepts/smart-contracts/understanding-hederas-evm-differences-and-compatibility/README.md).


## Learn More

## Security

## Contribute

## License

## Legal
