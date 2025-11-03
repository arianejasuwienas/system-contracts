> System contract address: `0x167` (unchanged)

# Hedera Token Service (HTS) v2 (ABI 0.3.0) — New & Changed System Contract Functions

| Function Name                      | Function Selector Hash |          Consensus Node Release Version | HIP        | Method Interface                                                                                                                    | Comments                                                                                                                                                                                              |
| ---------------------------------- | ---------------------: | --------------------------------------: | ---------- | ----------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cryptoTransfer` (v2)              |           `0x0e71804f` | *Unreleased in docs at time of writing* | [HIP 206]  | `cryptoTransfer(TransferList memory transferList, TokenTransferList[] memory tokenTransfers)`                                       | Atomic hbar+token transfers; replaces v1 single-arg form.                                                                                                                                             |
| `airdropTokens`                    |           `0x2f348119` |                                  0.56.0 | [HIP 904]  | `airdropTokens(TokenTransferList[] memory tokenTransfers)`                                                                          | Airdrop w/ pending claims & auto-association. ([Hiero Improvement Proposals][1])                                                                                                                      |
| `cancelAirdrops`                   |           `0x012ebcaf` |                                  0.56.0 | [HIP 904]  | `cancelAirdrops(PendingAirdrop[] memory pendingAirdrops) external returns (int64 responseCode)`                                     | Cancel unclaimed pending airdrops. ([Hiero Improvement Proposals][1])                                                                                                                                 |
| `claimAirdrops`                    |           `0x05961641` |                                  0.56.0 | [HIP 904]  | `claimAirdrops(PendingAirdrop[] memory pendingAirdrops) external returns (int64 responseCode)`                                      | Receiver claims pending airdrops. ([Hiero Improvement Proposals][1])                                                                                                                                  |
| `rejectTokens`                     |           `0xebd595e0` |                                  0.56.0 | [HIP 904]  | `rejectTokens(address rejectingAddress, address[] memory ftAddresses, NftID[] memory nftIDs) external returns (int64 responseCode)` | Return FTs/NFTs to treasury; no custom fees charged. ([Hiero Improvement Proposals][1])                                                                                                               |
| `updateFungibleTokenCustomFees`    |           `0xe780c5d3` |                                  0.54.2 | [HIP 1010] | `updateFungibleTokenCustomFees(address token, FixedFee[] memory fixedFees, FractionalFee[] memory fractionalFees)`                  | Update fee schedule for FTs via contracts. ([Hiero Improvement Proposals][2])                                                                                                                         |
| `updateNonFungibleTokenCustomFees` |           `0x01f9eb7d` |                                  0.54.2 | [HIP 1010] | `updateNonFungibleTokenCustomFees(address token, FixedFee[] memory fixedFees, RoyaltyFee[] memory royaltyFees)`                     | Update fee schedule for NFTs via contracts. ([Hiero Improvement Proposals][2])                                                                                                                        |
| `updateNFTsMetadata`               |           `0x0fcaca1f` |                            *(see note)* | [HIP 1028] | `updateNFTsMetadata(address nftToken, int64[] memory serialNumbers, bytes memory metadata) external returns (int responseCode)`     | Contract-driven per-serial NFT metadata updates; part of metadata-key work. *(Lives under a broader metadata track; rollout details may vary by network version.)* ([Hiero Improvement Proposals][3]) |

**Notes**

* ABI 0.3.0 also adds `isApproval` flags to transfer structs (`AccountAmount`, `NftTransfer`) and introduces the two-argument `cryptoTransfer` for atomic HBAR + token moves.
* The rest of the **ABIv1** convenience methods (e.g., `transferToken`, `transferTokens`, `transferNFT`, `approve`, `transferFromNFT`, `setApprovalForAll`, `isApprovedForAll`, etc.) remain available with their existing selectors from your original table. ([docs.hedera.com][4])

---

# Facade (EOA) Functions added with Airdrops

| Function Name      | Function Selector Hash | Consensus Node Release Version | HIP       | Method Interface                                                                                      | Comments                                                                              |
| ------------------ |------------------------|--------------------------------| --------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `cancelAirdropFT`  | `0xcef5b705`           | 0.56.0                         | [HIP 904] | `cancelAirdropFT(address receiverAddress) external returns (int64 responseCode)`                      | Sender cancels pending FT airdrop. ([Hiero Improvement Proposals][1])                 |
| `cancelAirdropNFT` | `0xad4917cf`           | 0.56.0                         | [HIP 904] | `cancelAirdropNFT(address receiverAddress, int64 serialNumber) external returns (int64 responseCode)` | Sender cancels pending NFT airdrop. ([Hiero Improvement Proposals][1])                |
| `claimAirdropFT`   | `0xa83bc5b2`           | 0.56.0                         | [HIP 904] | `claimAirdropFT(address senderAddress) external returns (int64 responseCode)`                         | Receiver claims FT. ([Hiero Improvement Proposals][1])                                |
| `claimAirdropNFT`  | `0x63ada5d7`           | 0.56.0                         | [HIP 904] | `claimAirdropNFT(address senderAddress, int64 serialNumber) external returns (int64 responseCode)`    | Receiver claims NFT. ([Hiero Improvement Proposals][1])                               |
| `rejectTokenFT`    | `0x76c6b391`           | 0.56.0                         | [HIP 904] | `rejectTokenFT() external returns (int64 responseCode)`                                               | EOA rejects all balance of a given FT (via proxy). ([Hiero Improvement Proposals][1]) |
| `rejectTokenNFTs`  | `0xa869c78a`           | 0.56.0                         | [HIP 904] | `rejectTokenNFTs(int64[] memory serialNumbers) external returns (int64 responseCode)`                 | EOA rejects specific NFT serials (via proxy). ([Hiero Improvement Proposals][1])      |

---

## Quick migration hints (from your v1 table)

* If you were calling `cryptoTransfer(TokenTransferList[])`, switch to **v2** when you need atomic HBAR + token moves: `cryptoTransfer(TransferList, TokenTransferList[])`. Otherwise the v1 form still exists.
* To support frictionless airdrops and user-initiated claims/rejections from DApps, wire in the four **airdrop** methods plus `rejectTokens` (or the EOA facade where appropriate). ([Hiero Improvement Proposals][1])
* Updating **custom fees** from contracts requires the two HIP-1010 methods above (released in **v0.54.2**). ([Hiero Improvement Proposals][2])
* If you expose **dynamic NFT metadata**, use `updateNFTsMetadata` and ensure your token has a **metadata key** configured; see HIP-1028 (note it also proposes a future **0x16c** address for a metadata-aware HTS variant). ([Hiero Improvement Proposals][3])

---

**Sources / Specs**

* Hedera docs: HTS system contract methods and versions.
* HIP-904 (Frictionless Airdrops): selectors, facades, release **v0.56.0**. ([Hiero Improvement Proposals][1])
* HIP-1010 (Update Token Custom Fees via Smart Contracts): selectors, release **v0.54.2**. ([Hiero Improvement Proposals][2])
* HIP-1028 (Metadata management via SmartContracts): `updateNFTsMetadata` selector and metadata-key context. ([Hiero Improvement Proposals][3])

[1]: https://hips.hedera.com/HIP/hip-904.html "HIP-904: Frictionless Airdrops"
[2]: https://hips.hedera.com/HIP/hip-1010.html "HIP-1010: Update Token Custom Fee Schedules via Smart Contracts"
[3]: https://hips.hedera.com/HIP/hip-1028.html "HIP-1028: Metadata management via SmartContracts"
[4]: https://docs.hedera.com/hedera/core-concepts/smart-contracts/tokens-managed-by-smart-contracts/hedera-token-service-system-contract?utm_source=chatgpt.com "Hedera Token Service System Contract"
