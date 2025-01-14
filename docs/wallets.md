# Wallets

You should instantiate a Wallet controller in order to manage wallets.
```js
const walletController = sdk.getWalletController()
```

#### `walletController.generateRandomMnemonic(strength)`

Generate random mnemonic (words) to generate wallets.
* `strength` (number)(__optional__) - strength number.

```js
const mnemonic = walletController.generateRandomMnemonic()
console.log(mnemonic)
// "window license ordinary apple toilet wrestle disease sudden until armor wealth room..."
```

#### `walletController.generateWallet(opts)`

Generate a wallet for a blockchain protocol:
* `opts.protocol` (string) (__required__) - blockchain protocol supported: `BITCOIN`, `ETHEREUM`, `BSC`, `CELO`, `STELLAR`, `RIPPLE`, `HATHOR`, `CARDANO`, `AVAXCCHAIN`, `SOLANA`.
* `opts.mnemonic` (string) (__optional__) - mnemonic string to generate private and public keys.
* `opts.derivation` (object) (__optional__) - derivation object (BIP44 derivation path).
* `opts.derivation.account` (number) (__optional__) - derivation account index.
* `opts.derivation.change` (number) (__optional__) - derivation change index.
* `opts.derivation.address` (number) (__optional__) - derivation address index.

Example:
```js
// generate random wallet for blockchain protocol
const wallet = await walletController.generateWallet({ protocol: 'STELLAR' })

// or using an existing mnemonic
const wallet = await walletController.generateWallet({
  protocol: 'ETHEREUM',
  mnemonic: '<words>...',
  derivation: { account: 10, address: 2 }
})
console.log(wallet)
// Wallet {
//   privateKey: '...',
//   publicKey: '...',
//   xpub: '...',
//   protocol: 'ETHEREUM',
// }
```

#### `walletController.generateWalletFromPrivateKey(opts)`

Generate a wallet for a blockchain protocol:
* `opts.protocol` (string) (__required__) - blockchain protocol supported: `BITCOIN`, `ETHEREUM`, `BSC`, `CELO`, `STELLAR`, `RIPPLE`, `HATHOR`, `CARDANO`, `AVAXCCHAIN`,`SOLANA`.
* `opts.privateKey` (__required__)
  * (string) - most protocols (`BITCOIN`, `ETHEREUM`, `BSC`, `CELO`, `STELLAR`, `RIPPLE`, `HATHOR`, `AVAXCCHAIN`,`SOLANA`) require a private key string.
  * (object) - the `CARDANO` protocol requires an object containing two different private keys
    * `opts.privateKey.spendingPrivateKey` (string) - private key used for spending operations
    * `opts.privateKey.stakingPrivateKey` (string) - private key used for staking operations

Example:
```js
// using an existing private key
const wallet = await walletController.generateWalletFromPrivateKey({
  privateKey: '0x...',
  protocol: 'BSC',
})

// for the cardano network
const cardanoWallet = await walletController.generateWalletFromPrivateKey({
  privateKey: {
    spendingPrivateKey: '0x...',
    stakingPrivateKey: '0x...',
    },
  protocol: 'CARDANO',
})
```

#### `walletController.generateWalletAddressFromXpub(opts)`

Generate a wallet address from xpub for a blockchain protocol:
* `opts.protocol` (string) (__required__) - blockchain protocol supported: `BITCOIN`, `ETHEREUM`, `BSC`, `CELO`, `HATHOR`, `CARDANO`,`AVAXCCHAIN`.
* `opts.xpub` (string) (__required__) - xpub string.
* `opts.address` (number) (__required__) - address index.

Example:
```js
const walletAddress = await walletController.generateWalletAddressFromXpub({
  xpub: 'xpub...',
  protocol: 'BSC',
  address: 0
})
```
