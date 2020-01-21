# useWallet()

## Introduction

useWallet() allows dapp users to connect to the provider of their choice in a
way that is as straightforward as possible. It provides an identical data
structure for the connected account, no matter what provider has been chosen by
the user. It aims to provide the features that are constantly reimplemented in
the context of a dapp.

### Features

- All-in-one solution to connect to Ethereum providers.
- Completely library agnostic (use Web3.js, ethers.js, …).
- Provides the current balance.
- Keeps track of the recent transactions.

### Opinionated?

Oh yes:

- React only.
- Exports as ES Modules.
- Ethereum only (for now).
- Supports one network at a time.
- Embeds as many providers as possible.
- Every prop and parameter is optional.

### What useWallet() is not

- An Ethereum wallet implementation.
- A low-level ethereum provider connector.
- An library to interact with Ethereum.

### Special thanks

useWallet() is a built on
[web3-react](https://github.com/NoahZinsmeister/web3-react) and its connectors,
which are doing all the hard work internally.

## Usage

```console
yarn add use-wallet
```

```jsx
import { useWallet, UseWalletProvider } from 'use-wallet'

function MyApp() {
  const { account, balance, connected, activate, deactivate } = useWallet()

  return (
    <>
      <h1>Wallet</h1>
      {connected ? (
        <div>
          <div>Account: {account}</div>
          <div>Balance: {balance}</div>
          <button onClick={() => deactivate()}>disconnect</button>
        </div>
      ) : (
        <div>
          Connect:
          <button onClick={() => activate()}>MetaMask</button>
          <button onClick={() => activate('frame')}>Frame</button>
          <button onClick={() => activate('portis')}>Portis</button>
        </div>
      )}
    </>
  )
}

// Wrap your app in UseWalletProvider
ReactDOM.render(
  <UseWalletProvider>
    <MyApp />
  </UseWalletProvider>,
  document.querySelector('#app')
)
```

## API

### <UseWalletProvider />

#### chainId

The [Chain ID](https://chainid.network/) supported by the connection. Defaults to 1.

#### connectors

The configuration of the different connectors. If a connector that requires a configuration gets used without, an error will be thrown.

- `injected`: no configuration needed.
- `frame`: no configuration needed.
- `fortmatic`: `{ apiKey }`
- `portis`: `{ dAppId }`
- `authereum`: no configuration needed.
- `squarelink`: `{ clientId, options }`
- `torus`: `{ initOptions, constructorOptions }`
- `walletlink`: `{ url, appName, appLogoUrl }`

See the [web3-react documentation](https://github.com/NoahZinsmeister/web3-react/tree/v6/docs) for more details.

### useWallet()

Returns an object representing the connected account (“wallet”), containing:

- `account`: the address of the account, or `null` when disconnected.
- `activate(connectorId)`: call this function with a connector ID to “connect” to a provider (see above for the connectors provided by default).
- `balance`: the balance of the account, in wei.
- `connected`: whether the account is connected or not (same as testing `account !== null`).
- `connectors`: the full list of connectors.
- `deactivate()`: call this function to “disconnect” from the current provider.
- `networkName`: a human-readable name corresponding to the Chain ID.