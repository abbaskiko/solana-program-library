---
title: Token Program
---

A Token program on the Solana blockchain.

This program defines a common implementation for Fungible and Non Fungible tokens.

## Background

Solana's programming model and the definitions of the Solana terms used in this
document are available at:

- https://docs.solana.com/apps
- https://docs.solana.com/terminology

## Source

The Token Program's source is available on
[github](https://github.com/solana-labs/solana-program-library)

## Interface

The Token Program is written in Rust and available on [crates.io](https://crates.io/crates/spl-token) and [docs.rs](https://docs.rs/spl-token).

Auto-generated C bindings are also available
[here](https://github.com/solana-labs/solana-program-library/blob/master/token/program/inc/token.h)

[JavaScript
bindings](https://github.com/solana-labs/solana-program-library/blob/master/token/js/client/token.js)
are available that support loading the Token Program on to a chain and issue
instructions.

See the [SPL Associated Token Account](associated-token-account.md) program for
convention around wallet address to token account mapping and funding.

## Command-line Utility

The `spl-token` command-line utility can be used to experiment with SPL
tokens.  Once you have [Rust installed](https://rustup.rs/), run:
```sh
$ cargo install spl-token-cli
```

Run `spl-token --help` for a full description of available commands.

### Configuration

The `spl-token` configuration is shared with the `solana` command-line tool.

#### Current Configuration

```
solana config get
```

```
Config File: ${HOME}/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com
WebSocket URL: wss://api.mainnet-beta.solana.com/ (computed)
Keypair Path: ${HOME}/.config/solana/id.json
```

#### Cluster RPC URL

See [Solana clusters](https://docs.solana.com/clusters) for cluster-specific RPC URLs
```
solana config set --url https://devnet.solana.com
```

#### Default Keypair

See [Keypair conventions](https://docs.solana.com/cli/conventions#keypair-conventions)
for information on how to setup a keypair if you don't already have one.

Keypair File
```
solana config set --keypair ${HOME}/new-keypair.json
```

Hardware Wallet URL (See [URL spec](https://docs.solana.com/wallet-guide/hardware-wallets#specify-a-keypair-url))
```
solana config set --keypair usb://ledger/
```

### Example: Creating your own fungible token

```sh
$ spl-token create-token
Creating token AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
Signature: 47hsLFxWRCg8azaZZPSnQR8DNTRsGyPNfUK7jqyzgt7wf9eag3nSnewqoZrVZHKm8zt3B6gzxhr91gdQ5qYrsRG4
```

The unique identifier of the token is `AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM`.

Tokens when initially created by `spl-token` have no supply:
```sh
spl-token supply AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
0
```

Let's mint some.  First create an account to hold a balance of the new
`AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM` token:
```sh
$ spl-token create-account AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
Creating account 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
Signature: 42Sa5eK9dMEQyvD9GMHuKxXf55WLZ7tfjabUKDhNoZRAxj9MsnN7omriWMEHXLea3aYpjZ862qocRLVikvkHkyfy
```

`7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi` is now an empty account:
```sh
$ spl-token balance 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
0
```

Mint 100 tokens into the account:
```sh
$ spl-token mint AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 100
Minting 100 tokens
  Token: AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
  Recipient: 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
Signature: 41mARH42fPkbYn1mvQ6hYLjmJtjW98NXwd6pHqEYg9p8RnuoUsMxVd16RkStDHEzcS2sfpSEpFscrJQn3HkHzLaa
```

The token `supply` and account `balance` now reflect the result of minting:
```sh
$ spl-token supply AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
100
$ spl-token balance 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
100
```

### Example: View all Tokens that you own

```sh
$ spl-token accounts
Account                                      Token                                        Balance
-------------------------------------------------------------------------------------------------
2ryb53FGVLVYFXmAemN7avawevuNFXwTVetMpH9ag3XZ 7e2X5oeAAJyUTi4PfSGXFLGhyPw2H8oELm1mx87ZCgwF 84
7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 100
CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 0
JAopo117aj6HMwCRjXSyNpZfGDJRi7ukqHgs2inXD8Rc AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 0
```

### Example: Wrapping SOL in a Token

```sh
$ spl-token wrap 1
Wrapping 1 SOL into GJTxcnA5Sydy8YRhqvHxbQ5QNsPyRKvzguodQEaShJje
Signature: 4f4s5QVMKisLS6ihZcXXPbiBAzjnvkBcp2A7KKER7k9DwJ4qjbVsQBKv2rAyBumXC1gLn8EJQhwWkybE4yJGnw2Y
```

To unwrap the Token back to SOL:
```
$ spl-token unwrap GJTxcnA5Sydy8YRhqvHxbQ5QNsPyRKvzguodQEaShJje
Unwrapping GJTxcnA5Sydy8YRhqvHxbQ5QNsPyRKvzguodQEaShJje
  Amount: 1 SOL
  Recipient: vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
Signature: f7opZ86ZHKGvkJBQsJ8Pk81v8F3v1VUfyd4kFs4CABmfTnSZK5BffETznUU3tEWvzibgKJASCf7TUpDmwGi8Rmh
```

### Example: Transferring tokens to another user
First the receiver uses `spl-token create-account` to create their associated
token account for the Token type.  Then the receiver obtains their wallet
address by running `solana address` and provides it to the sender.

The sender then runs:
```
$ spl-token transfer 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi 50 vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
Transfer 50 tokens
  Sender: 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
  Recipient: vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
  Recipient associated token account: F59618aQB8r6asXeMcB9jWuY6NEx1VduT9yFo1GTi1ks

Signature: 5a3qbvoJQnTAxGPHCugibZTbSu7xuTgkxvF4EJupRjRXGgZZrnWFmKzfEzcqKF2ogCaF4QKVbAtuFx7xGwrDUcGd
```

### Example: Transferring tokens to another user, with sender-funding
If the receiver does not yet have an associated token account, the sender may
choose to fund the receiver's account.

The receiver obtains their wallet address by running `solana address` and provides it to the sender.

The sender then runs to fund the receiver's associated token account, at the
sender's expense, and then transfers 50 tokens into it:
```
$ spl-token transfer --fund-recipient 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi 50 vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
Transfer 50 tokens
  Sender: 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
  Recipient: vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
  Recipient associated token account: F59618aQB8r6asXeMcB9jWuY6NEx1VduT9yFo1GTi1ks
  Funding recipient: F59618aQB8r6asXeMcB9jWuY6NEx1VduT9yFo1GTi1ks (0.00203928 SOL)

Signature: 5a3qbvoJQnTAxGPHCugibZTbSu7xuTgkxvF4EJupRjRXGgZZrnWFmKzfEzcqKF2ogCaF4QKVbAtuFx7xGwrDUcGd
```

### Example: Transferring tokens to an explicit recipient token account
Tokens may be transferred to a specific recipient token account.  The recipient
token account must already exist and be of the same Token type.

```
$ spl-token create-account AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
Creating account CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ
Signature: 4yPWj22mbyLu5mhfZ5WATNfYzTt5EQ7LGzryxM7Ufu7QCVjTE7czZdEBqdKR7vjKsfAqsBdjU58NJvXrTqCXvfWW
```
```
$ spl-token accounts AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
Account                                      Token                                        Balance
-------------------------------------------------------------------------------------------------
7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 100
CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 0
```
```
$ spl-token transfer 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi 50 CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ
Transfer 50 tokens
  Sender: 7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi
  Recipient: CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ

Signature: 5a3qbvoJQnTAxGPHCugibZTbSu7xuTgkxvF4EJupRjRXGgZZrnWFmKzfEzcqKF2ogCaF4QKVbAtuFx7xGwrDUcGd
```
```
$ spl-token accounts AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM
Account                                      Token                                        Balance
-------------------------------------------------------------------------------------------------
7UX2i7SucgLMQcfZ75s3VXmZZY4YRUyJN9X1RgfMoDUi AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 50
CqAxDdBRnawzx9q4PYM3wrybLHBhDZ4P6BTV13WsRJYJ AQoKYV7tYpTrFZN6P5oUufbQKAUr9mNYGe1TTJC9wajM 50
```

### Example: Create a non-fungible token

Create the token type,
```
$ spl-token create-token
Creating token 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
Signature: 4kz82JUey1B9ki1McPW7NYv1NqPKCod6WNptSkYqtuiEsQb9exHaktSAHJJsm4YxuGNW4NugPJMFX9ee6WA2dXts
```

then create an account to hold tokens of this new type:
```
$ spl-token create-account 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
Creating account 7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM
Signature: sjChze6ecaRtvuQVZuwURyg6teYeiH8ZwT6UTuFNKjrdayQQ3KNdPB7d2DtUZ6McafBfEefejHkJ6MWQEfVHLtC
```

Now mint only one token into the account,
```
$ spl-token mint 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z 1 7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM
Minting 1 tokens
  Token: 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
  Recipient: 7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM
Signature: 2Kzg6ZArQRCRvcoKSiievYy3sfPqGV91Whnz6SeimhJQXKBTYQf3E54tWg3zPpYLbcDexxyTxnj4QF69ucswfdY
```

and disable future minting:
```
$ spl-token authorize 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z mint --disable
Updating 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
  Current mint authority: vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
  New mint authority: disabled
Signature: 5QpykLzZsceoKcVRRFow9QCdae4Dp2zQAcjebyEWoezPFg2Np73gHKWQicHG1mqRdXu3yiZbrft3Q8JmqNRNqhwU
```

Now the `7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM` account holds the
one and only `559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z` token:

```
$ spl-token account-info 7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM

Address: 7KqpRwzkkeweW5jQoETyLzhvs9rcCj9dVQ1MnzudirsM
Balance: 1
Mint: 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
Owner: vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg
State: Initialized
Delegation: (not set)
Close authority: (not set)
```

```
$ spl-token supply 559u4Tdr9umKwft3yHMsnAxohhzkFnUBPAFtibwuZD9z
1
```

## JSON RPC methods

There is a rich set of JSON RPC methods available for use with SPL Token:
* `getTokenAccountBalance`
* `getTokenAccountsByDelegate`
* `getTokenAccountsByOwner`
* `getTokenLargestAccounts`
* `getTokenSupply`

See https://docs.solana.com/apps/jsonrpc-api for more details.

Additionally the versatile `getProgramAcccounts` JSON RPC method can be employed in various ways to fetch SPL Token accounts of interest.

### Finding all token accounts for a specific mint

To find all token accounts for the `TESTpKgj42ya3st2SQTKiANjTBmncQSCqLAZGcSPLGM` mint:
```
curl http://api.mainnet-beta.solana.com -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getProgramAccounts",
    "params": [
      "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
      {
        "encoding": "jsonParsed",
        "filters": [
          {
            "dataSize": 165
          },
          {
            "memcmp": {
              "offset": 0,
              "bytes": "TESTpKgj42ya3st2SQTKiANjTBmncQSCqLAZGcSPLGM"
            }
          }
        ]
      }
    ]
  }
'
```

The `"dataSize": 165` filter selects all [Token
Acccount](https://github.com/solana-labs/solana-program-library/blob/08d9999f997a8bf38719679be9d572f119d0d960/token/program/src/state.rs#L86-L106)s,
and then the `"memcmp": ...` filter selects based on the
[mint](https://github.com/solana-labs/solana-program-library/blob/08d9999f997a8bf38719679be9d572f119d0d960/token/program/src/state.rs#L88)
address within each token account.

### Finding all token accounts for a wallet

Find all token accounts owned by the `vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg` user:
```
curl http://api.mainnet-beta.solana.com -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getProgramAccounts",
    "params": [
      "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
      {
        "encoding": "jsonParsed",
        "filters": [
          {
            "dataSize": 165
          },
          {
            "memcmp": {
              "offset": 32,
              "bytes": "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg"
            }
          }
        ]
      }
    ]
  }
```

The `"dataSize": 165` filter selects all [Token
Acccount](https://github.com/solana-labs/solana-program-library/blob/08d9999f997a8bf38719679be9d572f119d0d960/token/program/src/state.rs#L86-L106)s,
and then the `"memcmp": ...` filter selects based on the
[owner](https://github.com/solana-labs/solana-program-library/blob/08d9999f997a8bf38719679be9d572f119d0d960/token/program/src/state.rs#L90)
address within each token account.

## Operational overview

### Creating a new token type

A new token type can be created by initializing a new Mint with the
`InitializeMint` instruction. The Mint is used to create or "mint" new tokens,
and these tokens are stored in Accounts. A Mint is associated with each
Account, which means that the total supply of a particular token type is equal
to the balances of all the associated Accounts.

It's important to note that the `InitializeMint` instruction does not require
the Solana account being initialized also be a signer. The `InitializeMint`
instruction should be atomically processed with the system instruction that
creates the Solana account by including both instructions in the same
transaction.

Once a Mint is initialized, the `mint_authority` can create new tokens using the
`MintTo` instruction.  As long as a Mint contains a valid `mint_authority`, the
Mint is considered to have a non-fixed supply, and the `mint_authority` can
create new tokens with the `MintTo` instruction at any time.  The `SetAuthority`
instruction can be used to irreversibly set the Mint's authority to `None`,
rendering the Mint's supply fixed.  No further tokens can ever be Minted.

Token supply can be reduced at any time by issuing a `Burn` instruction which
removes and discards tokens from an Account.

### Creating accounts

Accounts hold token balances and are created using the `InitializeAccount`
instruction. Each Account has an owner who must be present as a signer in some
instructions.

An Account's owner may transfer ownership of an account to another using the
`SetAuthority` instruction.

It's important to note that the `InitializeAccount` instruction does not require
the Solana account being initialized also be a signer. The `InitializeAccount`
instruction should be atomically processed with the system instruction that
creates the Solana account by including both instructions in the same
transaction.

### Transferring tokens
Balances can be transferred between Accounts using the `Transfer` instruction.
The owner of the source Account must be present as a signer in the `Transfer`
instruction when the source and destination accounts are different.

It's important to note that when the source and destination of a `Transfer` are
the **same**, the `Transfer` will _always_ succeed. Therefore, a successful `Transfer`
does not necessarily imply that the involved Accounts were valid SPL Token
accounts, that any tokens were moved, or that the source Account was present as
a signer. We strongly recommend that developers are careful about checking that
the source and destination are **different** before invoking a `Transfer`
instruction from within their program.

### Burning

The `Burn` instruction decreases an Account's token balance without transferring
to another Account, effectively removing the token from circulation permanently.

There is no other way to reduce supply on chain. This is similar to transferring
to an account with unknown private key or destroying a private key. But the act
of burning by using `Burn` instructions is more explicit and can be confirmed on
chain by any parties.

Note: there is a method by which a malicious and determined account owner
can silently burn their tokens without updating supply on chain by making an
account that is removed by rent collection because of [this known issue](#rent-exemption-loophole).

### Authority delegation

Account owners may delegate authority over some or all of their token balance
using the `Approve` instruction. Delegated authorities may transfer or burn up
to the amount they've been delegated. Authority delegation may be revoked by the
Account's owner via the `Revoke` instruction.

### Multisignatures

M of N multisignatures are supported and can be used in place of Mint
authorities or Account owners or delegates. Multisignature authorities must be
initialized with the `InitializeMultisig` instruction. Initialization specifies
the set of N public keys that are valid and the number M of those N that must be
present as instruction signers for the authority to be legitimate.

It's important to note that the `InitializeMultisig` instruction does not
require the Solana account being initialized also be a signer. The
`InitializeMultisig` instruction should be atomically processed with the system
instruction that creates the Solana account by including both instructions in
the same transaction.

### Freezing accounts

The Mint may also contain a `freeze_authority` which can be used to issue
`FreezeAccount` instructions that will render an Account unusable.  Token
instructions that include a frozen account will fail until the Account is thawed
using the `ThawAccount` instruction.  The `SetAuthority` instruction can be used
to change a Mint's `freeze_authority`.  If a Mint's `freeze_authority` is set to
`None` then account freezing and thawing is permanently disabled and all
currently frozen accounts will also stay frozen permanently.

### Wrapping SOL

The Token Program can be used to wrap native SOL. Doing so allows native SOL to
be treated like any other Token program token type and can be useful when being
called from other programs that interact with the Token Program's interface.

Accounts containing wrapped SOL are associated with a specific Mint called the
"Native Mint" using the public key
`So11111111111111111111111111111111111111112`.

These accounts have a few unique behaviors

- `InitializeAccount` sets the balance of the initialized Account to the SOL
  balance of the Solana account being initialized, resulting in a token balance
  equal to the SOL balance.
- Transfers to and from not only modify the token balance but also transfer an
  equal amount of SOL from the source account to the destination account.
- Burning is not supported
- When closing an Account the balance may be non-zero.

The Native Mint supply will always report 0, regardless of how much SOL is currently wrapped.

### Rent-exemption

To ensure a reliable calculation of supply, a consistency valid Mint, and
consistently valid Multisig accounts all Solana accounts holding a Account,
Mint, or Multisig must contain enough SOL to be considered [rent
exempt](https://docs.solana.com/implemented-proposals/rent)

#### Rent-exemption loophole

However note that there is currently a loophole to escape from the rent-exemption
rule. It is possible to create SPL Token accounts that are not rent exempt by
spoofing the Rent sysvar, since
[there are insufficient sysvar checks](https://github.com/solana-labs/solana/pull/13175)
in the program. This could be abused to burn tokens by transferring tokens to
a non-exempt Account that is subsequently rent-collected out of existence.

### Closing accounts

An account may be closed using the `CloseAccount` instruction. When closing an
Account, all remaining SOL will be transferred to another Solana account
(doesn't have to be associated with the Token Program). Non-native Accounts must
have a balance of zero to be closed.

### Non-Fungible tokens
An NTF is simply a token type where only a single token has been minted.

## Wallet Integration Guide
This section describes how to integrate SPL Token support into an existing
wallet supporting native SOL.  It assumes a model whereby the user has a single
system account as their **main wallet address** that they send and receive SOL
from.

Although all SPL Token accounts do have their own address on-chain, there's no
need to surface these additional addresses to the user.

There are two programs that are used by the wallet:
* SPL Token program: generic program that is used by all SPL Tokens
* [SPL Associated Token Account](associated-token-account.md) program: defines
  the convention and provides the mechanism for mapping the user's wallet
  address to the associated token accounts they hold.

### How to fetch and display token holdings
The [getTokenAccountsByOwner](https://docs.solana.com/apps/jsonrpc-api#gettokenaccountsbyowner)
JSON RPC method can be used to fetch all token accounts for a wallet address.

For each token mint, the wallet could have multiple token accounts: the
associated token account and/or other ancillary token accounts

By convention it is suggested that wallets roll up the balances from all token
accounts of the same token mint into a single balance for the user to shield the
user from this complexity.

See the [Garbage Collecting Ancillary Token Accounts](#garbage-collecting-ancillary-token-accounts)
section for suggestions on how the wallet should clean up ancillary token accounts on the user's behalf.

### Associated Token Account
Before the user can receive tokens, their associated token account must be created
on-chain, requiring a small amount of SOL to mark the account as rent-exempt.

There's no restriction on who can create a user's associated token account.  It
could either be created by the wallet on behalf of the user or funded by a 3rd
party through an airdrop campaign.

The creation process is described [here](associated-token-account.md#creating-an-associated-token-account).

#### Sample "Add Token" workflow
The user should first fund their associated token when they want to receive tokens of a certain type.

The wallet should provide a UI that allow the users to "add a token".
The user selects the kind of token, and is presented with information about how
much SOL it will cost to add the token.

Upon confirmation, the wallet creates the associated token type as the described
[here](associated-token-account.md#creating-an-associated-token-account).

#### Sample "Airdrop campaign" workflow
For each recipient wallet addresses, send a transaction containing:
1. Create the associated token account on the recipient's behalf.
2. Use `TokenInstruction::Transfer` to complete the transfer

#### Associated Token Account Ownership
⚠️ The wallet should never use `TokenInstruction::SetAuthority` to set the
`AccountOwner` authority of the associated token account to another address.

### Ancillary Token Accounts
At any time ownership of an existing SPL Token account may be assigned to the
user.  One way to accomplish this is with the
`spl-token authorize <TOKEN_ADDRESS> owner <USER_ADDRESS>` command.  Wallets
should be prepared to gracefully manage token accounts that they themselves did
not create for the user.

### Transferring Tokens Between Wallets
The preferred method of transferring tokens between wallets is to transfer into
associated token account of the recipient.

The recipient must provide their main wallet address to the sender.  The sender
then:
1. Derives the associated token account for the recipient
1. Fetches the recipient's associated token account over RPC and checks that it exists.
1. If the recipient's associated token account does not exist, the sender wallet may choose to first fund the recipient's wallet at their expense
1. Use `TokenInstruction::Transfer` to complete the transfer.

### Registry for token details
At the moment Token Mint addresses need to be hard coded by each wallet.  **Improving this situation is a work in progress.**

### Garbage Collecting Ancillary Token Accounts
Wallets should empty ancillary token accounts as quickly as practical by
transferring into the user's associated token account.  This effort serves two
purposes:
* If the user is the close authority for the ancillary account, the wallet can
  reclaim SOL for the user by closing the account.
* If the ancillary account was funded by a 3rd party, once the account is
  emptied that 3rd party may close the account and reclaim the SOL.

One natural time to garbage collect ancillary token accounts is when the user
next sends tokens.  The additional instructions to do so can be added to the
existing transaction, and will not require an additional fee.

Cleanup Pseudo Steps:
1. For all non-empty ancillary token accounts, add a
   `TokenInstruction::Transfer` instruction to the transfer the full token
   amount to the user's associated token account.
2. For all empty ancillary token accounts where the user is the close authority,
   add a `TokenInstruction::CloseAccount` instruction

If adding one or more of clean up instructions cause the transaction to exceed
the maximum allowed transaction size, remove those extra clean up instructions.
They can be cleaned up during the next send operation.

The `spl-token gc` command provides an example implementation of this cleanup process.
