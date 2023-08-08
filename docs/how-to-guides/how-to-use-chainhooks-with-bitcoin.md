---
title: Use Chainhooks with Bitcoin
---

The following guide helps you define predicates to use Chainhook with Bitcoin. The predicates are specified based on `if-this`, `then-that` constructs.

## `if_this` Specifications

Currently, `bitcoin` predicates support the following `if_this` constructs.

Get any transaction matching a given transaction ID (txid):

- `txid` mandatory argument admits:
  - 32 bytes hex encoded type.

```json

{
    "if_this": {
        "scope": "txid",
        "equals": "0xfaaac1833dc4883e7ec28f61e35b41f896c395f8d288b1a177155de2abd6052f"
    }
}
```

Get any transaction, including:

- OP_RETURN output starting with a set of characters.
  - `starts_with` mandatory argument admits:
    - ASCII string type. Example: `X2[`
    - hex encoded bytes. Example: `0x589403`

```json
{
    "if_this": {
        "scope": "outputs",
        "op_return": {
            "starts_with": "X2["
        }
    }
}
```

`op_return` is used to find blocks starting, ending, or equivalent to a specific string from the list of output blocks.

Get any transaction, including an OP_RETURN output matching the sequence of bytes specified:

- `equals` mandatory argument admits:
  - hex encoded bytes. Example: `0x69bd04208265aca9424d0337dac7d9e84371a2c91ece1891d67d3554bd9fdbe60afc6924d4b0773d90000006700010000006600012`

```json
{
    "if_this": {
        "scope": "outputs",
        "op_return": {
            "equals": "0x69bd04208265aca9424d0337dac7d9e84371a2c91ece1891d67d3554bd9fdbe60afc6924d4b0773d90000006700010000006600012"
        }
    }
}
```

Get any transaction, including an OP_RETURN output ending with a set of characters:

- `ends_with` mandatory argument admits:
  - ASCII string type. Example: `X2[`
  - hex encoded bytes. Example: `0x76a914000000000000000000000000000000000000000088ac`

```json
{
    "if_this": {
        "scope": "outputs",
        "op_return": {
            "ends_with": "0x76a914000000000000000000000000000000000000000088ac"
        }
    }
}
```

Get any transaction with a p2pkh output paying a given recipient:

- `p2pkh` construct admits:
  - ASCII string type. Example: "mr1iPkD9N3RJZZxXRk7xF9d36gffa6exNC"
  - hex encoded bytes type. Example: "0x76a914ee9369fb719c0ba43ddf4d94638a970b84775f4788ac"

```json
{
    "if_this": {
        "scope": "outputs",
        "p2pkh": {
            "equals": "mr1iPkD9N3RJZZxXRk7xF9d36gffa6exNC"
        }
    }
}
```

`p2pkh`(Pay-to-Public-Key-Hash) is a Bitcoin transaction output script type that allows users to send funds to a recipient's hashed public key, providing security and privacy by concealing the actual public key.

Get any transaction including a p2sh output paying a given recipient `p2sh` construct admits:

- `p2sh` construct admits:
  - string type. Example: "2MxDJ723HBJtEMa2a9vcsns4qztxBuC8Zb2"
  - hex encoded bytes type. Example: "0x76a914ee9369fb719c0ba43ddf4d94638a970b84775f4788ac"

```json
{
    "if_this": {
        "scope": "outputs",
        "p2sh": {
          "equals": "2MxDJ723HBJtEMa2a9vcsns4qztxBuC8Zb2"
        }
    }
}
```

`p2sh`(Pay-to-Script-Hash) is a Bitcoin transaction output script type that enables users to send funds to a script instead of a public key, allowing for more complex transaction conditions and multi-signature addresses.

Get any transaction, including a `p2wpkh` output paying a given recipient:

- `p2wpkh` construct admits:
  - string type. Example: "bcrt1qnxknq3wqtphv7sfwy07m7e4sr6ut9yt6ed99jg"

```json
{
    "if_this": {
        "scope": "outputs",
        "p2wpkh": {
          "equals": "bcrt1qnxknq3wqtphv7sfwy07m7e4sr6ut9yt6ed99jg"
        }
    }
}
```

`p2wpkh`(Pay-to-Witness-Public-Key-Hash) is a Bitcoin transaction output script type used in Segregated Witness (SegWit) that allows for more efficient and secure transactions by separating the witness data from the transaction data and storing it in a separate block.

Get any transaction, including a `p2wsh` output paying a given recipient:

- `p2wsh` construct admits:
  - string type. Example: "bc1qklpmx03a8qkv263gy8te36w0z9yafxplc5kwzc"

```json
{
    "if_this": {
        "scope": "outputs",
        "p2wsh": {
          "equals": "bc1qklpmx03a8qkv263gy8te36w0z9yafxplc5kwzc"
        }
    }
}
```

`p2wsh`  (Pay-to-Witness-Script-Hash) is a Bitcoin transaction output script type used in Segregated Witness (SegWit) that enables users to send funds to a hashed script, allowing for more complex transaction conditions and greater scalability by separating the script from the transaction data.

Get any Bitcoin transaction, including a Block commitment. Broadcasted payloads include _Proof of Transfer_ reward information:

```json
{
    "if_this": {
        "scope": "stacks_protocol",
        "operation": "block_committed"
    }
}
```

**Proof of Transfer(PoT)** is a blockchain consensus mechanism where participants prove ownership of assets outside the blockchain from another network and transfer them to the target blockchain to validate and secure transactions without relying solely on computational work or stake ownership.

Get any transaction, including a key registration operation:

```json
{
    "if_this": {
        "scope": "stacks_protocol",
        "operation": "leader_registered"
    }
}
```

Get any transaction, including an STX transfer operation:
// Coming soon

```json
{
    "if_this": {
        "scope": "stacks_protocol",
        "operation": "stx_transferred"
    }
}
```

Get any transaction, including an STX lock operation:
// Coming soon

```json
{
    "if_this": {
        "scope": "stacks_protocol",
        "operation": "stx_locked"
    }
}
```

Get any transaction including a new Ordinal inscription (inscription revealed and transferred)

```json
{
    "if_this": {
        "scope": "ordinals_protocol",
        "operation": "inscription_feed"
    }
}
```

## `then_that` Constructs

The following `then_that` constructs are supported:

`HTTP Post` block/transaction payload to a given endpoint:

- `http_post` construct admits:
  - url (string type). Example: http://localhost:3000/api/v1/wrapBtc
  - authorization_header (string type). Secret to add to the request `authorization` header when posting payloads

```jsonc

{
    "then_that": {
        "http_post": {
            "url": "http://localhost:3000/api/v1/wrapBtc",
            "authorization_header": "Bearer cn389ncoiwuencr"
        }
    }
}
```

Append events to a file through the filesystem. Convenient for local tests:

- `file_append` construct admits:
  - path (string type). Path to the file on disk.

```jsonc
{
    "then_that": {
        "file_append": {
            "path": "/tmp/events.json",
        }
    }
}
```

## Additional configuration knobs available

The following additional configurations can be used to improve the performance of Chainhook by preventing a full scan of the blockchain:

- Ignore any block before the given block:
`"start_block": 101`

- Ignore any block after the given block:
`"end_block": 201`

- Stop evaluating chainhook after a given number of occurrences found:
`"expire_after_occurrence": 1`

- Don't include proofs:
`"include_proof": false`

- Don't include Bitcoin transaction inputs in the payload:
`"include_inputs": false`

- Don't include Bitcoin transaction outputs in the payload:
`"include_outputs": false`

- Don't include Bitcoin transaction witnesses in the payload:
`"include_witness": false`

## Example predicate definition to post first five transfers

Retrieve and HTTP Post to `http://localhost:3000/api/v1/wrapBtc` the five first transfers to the p2wpkh `bcrt1qnxk...yt6ed99jg` address of any amount, occurring after block height 10200.

> [!NOTE]
> The start_block is mandatory to post events using `http_post` `then-that` predicate.

```json
{
  "chain": "bitcoin",
  "uuid": "1",
  "name": "Wrap BTC",
  "version": 1,
  "networks": {
    "testnet": {
      "if_this": {
        "scope": "outputs",
        "p2wpkh": {
          "equals": "bcrt1qnxknq3wqtphv7sfwy07m7e4sr6ut9yt6ed99jg"
        }
      },
      "then_that": {
        "http_post": {
          "url": "http://localhost:3000/api/v1/transfers",
          "authorization_header": "Bearer cn389ncoiwuencr"
        }
      },
      "start_block": 10200,
      "expire_after_occurrence": 5, // only prints the first 5 characters
    }
  }
}
```

## Example predicate with multiple networks

A specification file can also include different networks. In this case, the Chainhook will select the predicate corresponding to the network it was launched against. In the below example, the testnet and mainnet are two different networks.

```json
{
  "chain": "bitcoin",
  "uuid": "1",
  "name": "Wrap BTC",
  "version": 1,
  "networks": {
    "testnet": {
      "if_this": {
        "scope": "ordinals_protocol",
        "operation": "inscription_feed"
      },
      "then_that": {
        "http_post": {
          "url": "http://localhost:3000/api/v1/ordinals",
          "authorization_header": "Bearer cn389ncoiwuencr"
        }
      },
      "start_block": 10200,
    },
    "mainnet": {
      "if_this": {
        "scope": "ordinals_protocol",
        "operation": "inscription_feed"
      },
      "then_that": {
        "http_post": {
          "url": "http://my-protocol.xyz/api/v1/ordinals",
          "authorization_header": "Bearer cn389ncoiwuencr"
        }
      },
      "start_block": 90232,
    }

  }
}

```