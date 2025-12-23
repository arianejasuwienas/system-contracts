| Function Name                                          | Covered                                   |
|--------------------------------------------------------|-------------------------------------------|
| HederaTokenService.cryptoTransfer                      | OK                                        | 
| HederaTokenService.mintToken                           | without metadata                          | 
| HederaTokenService.burnToken                           | OK - Token management -> supply reduction | 
| HederaTokenService.associateTokens                     | OK                                        | 
| HederaTokenService.associateToken                      | OK                                        | 
| HederaTokenService.dissociateTokens                    | OK                                        | 
| HederaTokenService.dissociateToken                     | OK                                        | 
| HederaTokenService.createFungibleToken                 | without metadata                          | 
| HederaTokenService.createNonFungibleToken              | without metadata                          |
| HederaTokenService.createFungibleTokenWithCustomFee    | without metadata                          | 
| HederaTokenService.createNonFungibleTokenWithCustomFee | without metadata                          |
| HederaTokenService.getFungibleTokenInfo                | keys not listed                           |
| HederaTokenService.getTokenInfo                        | keys not listed                           |
| HederaTokenService.getNonFungibleTokenInfo             | keys not listed                           |
| HederaTokenService.getTokenCustomFees                  | fee label missing in response             |
| HederaTokenService.approve                             | OK                                        |
| HederaTokenService.transferFrom                        | OK                                        |
| HederaTokenService.transferFromNFT                     | OK                                        |
| HederaTokenService.allowance                           | OK                                        |
| HederaTokenService.approveNFT                          | OK                                        |
| HederaTokenService.getApproved                         | OK                                        |
| HederaTokenService.isFrozen                            | OK                                        |
| HederaTokenService.isKyc                               | OK                                        |
| HederaTokenService.freezeToken                         | OK                                        |
| HederaTokenService.unfreezeToken                       | OK                                        |
| HederaTokenService.grantTokenKyc                       | OK                                        |
| HederaTokenService.revokeTokenKyc                      | OK                                        |
| HederaTokenService.setApprovalForAll                   | OK                                        |
| HederaTokenService.isApprovedForAll                    | OK                                        |
| HederaTokenService.getTokenDefaultFreezeStatus         | ?                                         |
| HederaTokenService.getTokenDefaultKycStatus            | ?                                         |
| HederaTokenService.transferTokens                      | OK                                        |
| HederaTokenService.transferNFTs                        | OK                                        |
| HederaTokenService.transferToken                       | OK                                        |
| HederaTokenService.transferNFT                         | OK                                        |
| HederaTokenService.pauseToken                          | OK                                        |
| HederaTokenService.unpauseToken                        | OK                                        |
| HederaTokenService.wipeTokenAccount                    | OK                                        |
| HederaTokenService.wipeTokenAccountNFT                 | OK                                        |
| HederaTokenService.deleteToken                         | missing?                                  |
| HederaTokenService.updateTokenKeys                     | OK                                        |
| HederaTokenService.getTokenKey                         | OK                                        |
| HederaTokenService.isToken                             | OK   -> validate token                    |
| HederaTokenService.getTokenType                        | OK                                        |
| HederaTokenService.getTokenExpiryInfo                  | OK                                        |
| HederaTokenService.updateTokenExpiryInfo               | OK                                        |
| HederaTokenService.updateTokenInfo                     | OK                                        |
| HederaTokenService.redirectForToken                    | missing                                   |
| HederaTokenService.updateFungibleTokenCustomFee        | missing                                   |
| HederaTokenService.updateNonFungibleTokenCustomFee     | missing                                   |
| HederaTokenService.*Aidrop(s)                          | missing                                   |
| HederaTokenService.rejectTokens                        | missing                                   |
| HederaAccountService.*                                 | missing                                   |
| HederaScheduleService.*                                | missing                                   |
| PRNG.*                                                 | OK                                        |




1. Metadata support missing
2. Compressed public key: pk.public_key.format(compressed=True).hex()
    0x02b3c641418e89452cd5202adfd4758f459acb8e364f741fd16cd2db79835d39d2
TESTNET 0x0296b4ed417f7ce02a195d1c942f57656608e0f713b17de66ea7184cac1c3d0e59
   With 0x
Account: 0x292c4acf9ec49af888d4051eb4a4dc53694d1380
    Maybe calculate automatically?
3. Token Transfer Contract  0x67d8d32e9bf1a9968a5ff53b87d777aa8ebbee69 (ADDrESS, not ACCOUNTID)
4. Gas limits / prices not precaculated
5. Maybe prepare examples with structs constructed in FE?
6. Validation (see in console )


Got it — here’s a revised version of your **ADR**, keeping it short, neutral (no decision yet), and including the note about adding metadata, schedule service, and account service coverage:

---

# ADR: Support for dApp for System Contract interaction

**Date:** 2025-11-05
**Status:** Draft / Pending Decision
**Authors:** [Your Name]

---

## Context

A demo application exists that:

* Deploys *extension/translator* smart contracts, providing simplified interfaces for interacting with system contracts and precompiles.
* Allows users to test and call these extensions to observe how inputs are transformed into structs consumed by system contracts.

Allows users to test and call these extensions to observe the interactiions with system contracts and the functionalities they offer.


The demo app serves as a simple and illustrative showcase of how system contracts work, requiring little effort to start 
or understand.
However, current limitations include:

* Not all system contracts and functionalities are covered.
* Missing validations in some areas.
* Lack of user-friendly defaults (e.g., key creation).
* Manual gas/price specification is required, which may be non-trivial for users.

---

## Options Considered

### **Option 1 – Keep as-is**

**Description:**
Retain the current demo app without further development.

**Pros:**

* No additional effort or maintenance cost.
* Continues to serve as a simple reference for developers.

**Cons:**

* Usability issues remain (manual gas, lack of defaults).
* Limited coverage of system contracts and features.
* Might become outdated or misleading as system evolves.

---

### **Option 2 – Keep and Improve**

**Description:**
Keep the demo app but enhance it by:

* Adding better validation and user feedback.
* Introducing default values/presets (e.g., auto-generated keys, pre-calculated gas estimates).
* Expanding coverage to currently unsupported features (e.g., metadata) and additional contracts such as **Schedule Service** and **Account Service**.

**Pros:**

* Improves user and developer experience.
* Provides more complete showcase of system functionality.
* Relatively low-cost compared to a full rewrite.

**Cons:**

* Translator abstraction layer remains, which may not fully reflect real-world usage.
* Still requires ongoing maintenance to keep up with system contract changes.

---

### **Option 3 – Rewrite**

**Description:**
Rebuild the demo app to remove translator contracts. The client would construct and send structs directly to system contracts.
Also extend the scope to include unsupported features (metadata) and additional contracts (Schedule Service, Account Service).

**Pros:**

* Aligns more closely with production interaction patterns.
* Provides deeper, more accurate technical insight.
* Opportunity to redesign UX and architecture cleanly.

**Cons:**

* Requires significant development effort.
* More complex for new users to understand initially.
* Longer time to deliver improvements.

---

### **Option 4 – Remove / Drop**

**Description:**
Decommission the demo app entirely.

**Pros:**

* No maintenance or support effort.
* Reduces scope of documentation and testing.

**Cons:**

* Loss of a useful demonstration and educational tool.
* Makes it harder for new developers to explore system contract interactions.

---

## Next Steps

Evaluate trade-offs between effort and long-term value to decide whether to:

* Keep the demo as-is,
* Improve it incrementally,
* Rewrite it for direct contract interaction, or
* Remove it entirely.

---

Would you like me to add a short **“Decision Drivers”** section (e.g., developer onboarding, maintenance cost, realism of interactions) to make it more consistent with typical ADR structure?
