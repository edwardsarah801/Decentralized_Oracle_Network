# Stacks Oracle (STX Oracle)

A decentralized oracle contract for the Stacks blockchain that enables bringing off-chain data on-chain in a secure, verifiable way.

## Overview

STX Oracle is a smart contract written in Clarity that allows authorized data providers to submit price feeds and other off-chain data to the Stacks blockchain. The contract includes provider registration, authorization mechanisms, data validation, and aggregation functions to ensure reliable data from multiple sources.

## Features

- **Provider Management**: Register and authorize trusted data providers
- **Data Feed Configuration**: Create and manage different types of data feeds
- **Data Submission**: Allow authorized providers to submit data points
- **Data Aggregation**: Aggregate data from multiple providers to ensure reliability
- **Staleness Prevention**: Track data freshness and validate timestamps
- **Access Control**: Owner-based permissions for critical operations

## Contract Structure

### Key Components

1. **Oracle Providers**: Entities that submit data to the contract
2. **Data Feeds**: Specific data types/assets that providers can submit (e.g., ETH-USD price)
3. **Provider Authorization**: Mapping of which providers are authorized for which feeds
4. **Data Points**: Individual submissions from providers
5. **Aggregated Values**: Final consolidated values from multiple providers

### Data Flow

1. Contract owner creates data feeds with configuration parameters
2. Data providers register with the contract
3. Contract owner authorizes specific providers for specific feeds
4. Authorized providers submit data points
5. Contract aggregates data from multiple providers
6. Consumers can read the latest aggregated value

## Usage

### For Contract Owners

```clarity
;; Create a new data feed
(contract-call? .stx_oracle create-data-feed "ETH-USD" "Ethereum Price" "USD price of Ethereum" u3 u100)

;; Authorize a provider for a feed
(contract-call? .stx_oracle authorize-provider 'ST1SJ3DTE5DN7X54YDH5D64R3BCB6A2AG2ZQ8YPD5 "ETH-USD")
```

### For Data Providers

```clarity
;; Register as a provider
(contract-call? .stx_oracle register-provider "Chainlink")

;; Submit a data point (after being authorized)
(contract-call? .stx_oracle submit-data-point "ETH-USD" u200000 u1620000000)
```

### For Data Consumers

```clarity
;; Get the latest price for a feed
(contract-call? .stx_oracle get-latest-value "ETH-USD")

;; Check if the data is fresh
(contract-call? .stx_oracle is-data-fresh "ETH-USD")
```

## Error Codes

- `u100`: Owner-only operation
- `u101`: Not authorized
- `u102`: Not registered
- `u103`: Invalid value
- `u104`: Invalid timestamp
- `u105`: Stale data
- `u106`: Not enough providers

## Development and Testing

The project includes a comprehensive test suite built with Vitest that tests all main functionality of the contract.

### Running Tests

```bash
# Install dependencies
npm install

# Run tests
npm test
```

## Security Considerations

1. **Data Validation**: The contract validates timestamps to prevent future-dated submissions
2. **Multi-provider Aggregation**: Requires data from multiple providers to mitigate manipulation
3. **Staleness Checks**: Tracks data freshness to prevent usage of outdated information
4. **Access Control**: Only authorized providers can submit data for specific feeds

## Future Enhancements

- Add provider reputation scoring
- Implement more sophisticated aggregation methods (weighted median, trimmed mean)
- Support for multiple data types beyond numeric values
- Adding fee mechanisms for providers and consumers
- Implementing governance for decentralized management of the oracle system

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request