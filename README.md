# mywalleticon.com
This is the environment to bind beautiful icons to user wallets. We provide icon for any wallet. 
Our website is here https://mywalleticon.com/, you can try functionality there.

This is decentralized system, but we also offer simplified API to interact with environment.
Our smart-contracts are deployed on Etherium network. You can find source-codes and ABI files on etherscan.io

## IICO token contract
Link on etherscan.io https://etherscan.io/address/0x0C61A2A4ff97a1282e43Ea6C4e499e15ddF18064 
If user wants to customize icon or alias for their wallet they need to pay IICO tokens. IICO is ERC20 token 

## Vending contract
Link on etherscan.io https://etherscan.io/address/0xc68818cdd48ea3d0a288c44fcef602c1fea54047
User can exchange USDC to IICO using `buy(xtokens, receiver)` function:
 * xtokens is amount of USDC tokens to be exchanged (1 IICO costs 1 USDC)
 * receiver is the wallet address which receives IICO tokens

## IcoManager contract
Link on etherscan.io https://etherscan.io/address/0xdd876c52af74443d587c226d324aab26de2dc628
Most useful functions:
 * `getAlias(address)` returns alias for the address (if alias was set). Alias is represented as bytes32, every byte is ASCII symbol.
 * `getIconhash(skinId, address)` returns dag hash (SHA-256) for CIDv0 (see IPFS protocol) represented as bytes32. So far we have only one skin, use 0 as skinId. You can encode dag hash as CIDv0 and request icon from IPFS by yourself.

## Safety
 * We use only SVG icons. Although user is able to upload custom icon, it becomes public after the validation. Validator manually opens icon and makes sure it is correct SVG icon. After the validation any user can use uploaded icon.
 * Uploader takes responsibility for copyright violation.
 * Although user can set alias its value is strictly limited on the smart-contract lavel. Max length is 30 symbols, only A-Z, a-z, 0-9 and underline symbols are allowed. So you can not worry about any injections and display raw value.

## Backend API
Backend provides API to simplify integration for free, but free plan has limited bandwidth. 
 * GET https://mywalleticon.com/usericon/PUT_WALLET_ADDRESS_HERE reads smart-contract, reads IPFS file and returns image at once. So you can easily integrate it into your system.
 * GET https://mywalleticon.com/useralias/PUT_WALLET_ADDRESS_HERE reads smart-contract and returns JSON response with alias. Response structure:
```
{"code":"OK","details":null,"payload":"alias_is_here"}
```
 * GET https://mywalleticon.com/iconcid/PUT_CIDv0_HERE returns icon by CIDv0. NOTE: It works for registered CIDs only (dag hash must be registered and validated in the IcoManager contract). It can help you to simplify integration if you want to read icon hashes from the smart-contract directly
 * GET https://mywalleticon.com/iconhash/PUT_DAG_HASH_HERE returns icon by dag hash represented as hex string. NOTE: It works for registered dag hashes only (dag hash must be registered and validated in the IcoManager contract). It can help you to simplify integration if you want to read icon hashes from the smart-contract directly
 
For more details check this file: lightweight.html

## Decentralized integration
If you want to integrate it in decentralized way, here is the HowTo get wallet icon:
 * Request `iconhash` for the wallet from the smart-contract: `getIconhash(skinId, address)`. Use `0` as `skinId`. Smart-contract is deployed on `Eth` network, smart-contract address: `0xdd876c52af74443d587c226d324aab26de2dc628`. You can find ABI on etherscan: https://etherscan.io/address/0xdd876c52af74443d587c226d324aab26de2dc628#code
 * Encode `iconhash` into the `CidV0`. Prepend `iconhash` value with bytes `0x1220`. Encode these `34 bytes` as `Base58` string
 * Download SVG icon through the IPFS using `CidV0`

Example, 
 * Smart-contract returned to you the `iconhash: 0xa7e76719279416374a9122074751a2e2593a7305d8c85beb1f919c8cd33952cc`
 * Prepended hex-string: `0x1220a7e76719279416374a9122074751a2e2593a7305d8c85beb1f919c8cd33952cc`
 * Base58 string: `QmZe5Sz3j7qT4eGr6Fr9KJJqN89vFvna2JPn41ht1L96Dq`
 * Get icon through IPFS. [Example](https://gateway.pinata.cloud/ipfs/QmZe5Sz3j7qT4eGr6Fr9KJJqN89vFvna2JPn41ht1L96Dq)
 
For more details check this file: decentralized.html
