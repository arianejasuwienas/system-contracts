# Extensions

**What:**
A thin set of helper contracts that wrap Hedera system contracts (HTS and other) behind **simple, flat method signatures**.

**Why:**
Calling HTS directly often requires building nested **structs/objects** (`TransferList`, `TokenTransferList`, `KeyValue`, etc.). Extensions let you do the **same operations** (create/mint/burn/transfer/associate/airdrop, …) but with **primitive arguments only** (addresses, ints, bytes)-no manual struct assembly.

**How:**
Import the desired extension and call its methods with simple types. The extension converts your inputs into the correct HTS/system-contract structs and calls under the hood.

```solidity
// instead of constructing full Token struct yourself…
// must manually construct `HederaToken` with expiry, keys, etc.
function createFungibleToken(
    IHederaTokenService.HederaToken memory token,
    int64 initialTotalSupply,
    int32 decimals
) internal returns (int responseCode, address tokenAddress);

// extension composes HederaToken internally and calls HTS
function createFungibleTokenPublic(
    string memory name,
    string memory symbol,
    string memory memo,
    int64 initialTotalSupply,
    int64 maxSupply,
    int32 decimals,
    bool freezeDefaultStatus,
    address treasury,
    IHederaTokenService.TokenKey[] memory keys
) public payable {
}
```

**Benefits:**

* Same HTS functionality, simplified developer experience

**Use When:**

* You want quick, ergonomic interactions with HTS/system contracts
* You don’t need the full flexibility of low-level struct-based interfaces
