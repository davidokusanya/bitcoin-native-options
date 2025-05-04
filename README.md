# SatoshiOptions - Bitcoin-native Options Protocol

**SatoshiOptions** is a decentralized financial options protocol built for the **Stacks/Bitcoin ecosystem**. It enables trustless creation, trading, and settlement of options contracts using **BTC** and **STX** as collateral. With Bitcoin-level security and Stacks L2 programmability, it unlocks programmable, non-custodial derivatives on Bitcoin.

## Features

* **Trustless Options Contracts**: Support for fully on-chain **CALL** and **PUT** options with customizable parameters.
* **Collateralization**: Use SIP-010 compliant tokens like **BTC** and **STX** as collateral.
* **Built-in Price Oracle**: Ensures accurate and up-to-date pricing for fair option execution.
* **Secure Settlement**: Options are settled on-chain based on real-time oracle data.
* **User Position Tracking**: Track written and held options per user with automatic collateral accounting.
* **Governance Support**: Protocol owner can manage fee rates, approved tokens, and oracle symbols.

## How It Works

### 1. Write Option

Users lock collateral and define strike price, premium, expiry, and type (CALL or PUT). A unique option ID is generated.

```clarity
(write-option token collateral-amount strike-price premium expiry option-type)
```

### 2. Buy Option

Buyers pay the premium to the writer and receive rights to exercise the option before expiry.

```clarity
(buy-option token option-id)
```

### 3. Exercise Option

If market conditions are favorable, the holder can exercise the option and claim profits from locked collateral.

```clarity
(exercise-option token option-id)
```

## Data Structures

* `options`: Stores metadata for each option contract.
* `user-positions`: Tracks written and held options per user and total collateral.
* `approved-tokens`: Whitelist for valid SIP-010 tokens.
* `price-feeds`: Oracle-fed price data keyed by symbol (e.g., `BTC-USD`).
* `allowed-symbols`: Whitelisted symbols for price feeds.

## Admin Functions

* `set-protocol-fee-rate`: Adjust fee rate (max 10%).
* `update-price-feed`: Inject price data (admin-only).
* `set-approved-token`: Add/remove allowed tokens.
* `set-allowed-symbol`: Add/remove allowed symbols.

Only the contract owner (set at deployment) can invoke these administrative actions.

## Security Features

* **Authorization Checks**: Only option holders can exercise their contracts.
* **Collateral Verification**: Ensures adequate funds are locked upfront.
* **Safe Transfers**: Relies on SIP-010 standard to interact with tokens securely.
* **Critical Resource Protection**: Prevents removal of essential tokens/symbols (like BTC-USD).

## Error Codes

| Code    | Meaning                  |
| ------- | ------------------------ |
| `u1000` | Not authorized           |
| `u1001` | Insufficient balance     |
| `u1002` | Invalid expiry           |
| `u1003` | Invalid strike price     |
| `u1004` | Option not found         |
| `u1005` | Option expired           |
| `u1006` | Insufficient collateral  |
| `u1007` | Already exercised        |
| `u1008` | Invalid premium          |
| `u1009` | Invalid token            |
| `u1010` | Invalid symbol           |
| `u1011` | Invalid timestamp        |
| `u1012` | Invalid address          |
| `u1013` | Zero address not allowed |
| `u1014` | Empty symbol string      |

## Interface: SIP-010 Token Trait

SatoshiOptions uses tokens implementing [SIP-010](https://github.com/stacksgov/sips/blob/main/sips/sip-010/sip-010-fungible-token-standard.md) to handle collateral and premiums.

```clarity
(define-trait sip-010-trait ...)
```

## Testing & Deployment

To deploy or test:

1. Ensure you are using a Clarity-compatible development environment (e.g., Clarinet).
2. Load the contract and verify compilation.
3. Use test scripts or manual calls to simulate `write-option`, `buy-option`, and `exercise-option`.

## Future Enhancements

* Option liquidity pools & DEX integrations
* Insurance vaults for option writers
* Multi-token collateral support
* Automated Oracle integrations (e.g., via Chainlink bridges)

## Contributing

We welcome community feedback, testing, and audits. Submit pull requests or open issues to help improve SatoshiOptions.
