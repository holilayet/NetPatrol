# NetPatrol

NetPatrol is a Solidity smart contract for storing arbitrary data on-chain as key-value pairs, with Hardhat and ethers.js tooling around it: a deploy script, a CLI, a small REST API, and a frontend integration example.

The contract (`contracts/Netpatrol.sol`) is a minimal Ownable key-value store:

- `store(bytes32 key, bytes value)` — stores any bytes under a key, emitting a `DataStored` event.
- `retrieve(bytes32 key)` — reads back the stored bytes.
- The deployer is recorded as the contract `owner` (OpenZeppelin `Ownable`).

## Project structure

- `contracts/Netpatrol.sol` — the smart contract
- `scripts/deploy.js` — deploys the contract, saves the deployment info to `deployments/<network>.json`
- `scripts/interact.js` — store / retrieve values through Hardhat
- `scripts/cli.js` — `netpatrol-cli` command-line tool (`store`, `retrieve`, `info`)
- `scripts/api-server.js` — REST API (`/health`, `/store`, `/retrieve/:key`, `/contract`)
- `scripts/frontend-integration.js` — example client and React hook for frontends
- `test/Netpatrol.test.js` — contract tests

## Requirements

- Node.js 18+
- npm

## Setup

```bash
git clone https://github.com/holilayet/NetPatrol.git
cd NetPatrol
npm install
cp .env.example .env
```

Fill in `.env` with your own values (see `.env.example`): `PRIVATE_KEY`, `SEPOLIA_RPC_URL`, `ETHERSCAN_API_KEY`. Never commit a real `.env` file.

## Usage

```bash
npx hardhat compile          # compile the contract
npx hardhat test             # run the tests
npx hardhat node             # start a local chain

npm run deploy:local         # deploy to localhost
npm run deploy:sepolia       # deploy to Sepolia

# Store and retrieve through Hardhat
npx hardhat run scripts/interact.js store <key> <value>
npx hardhat run scripts/interact.js retrieve <key>

# Or use the CLI (after deploying, so deployments/<network>.json exists)
node scripts/cli.js store <key> <value>
node scripts/cli.js retrieve <key>
node scripts/cli.js info

# Or run the REST API (default http://localhost:3000)
npm run api
```

## Configuration

| Variable | Description |
| --- | --- |
| `PRIVATE_KEY` | Private key used to deploy and sign transactions |
| `SEPOLIA_RPC_URL` | Sepolia RPC endpoint |
| `ETHERSCAN_API_KEY` | Etherscan API key for contract verification |
| `RPC_URL` | RPC endpoint for the CLI / API server (default `http://127.0.0.1:8545`) |
| `NETWORK` | Network name used by the API server (default `localhost`) |
| `PORT` | Port for the API server (default `3000`) |

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
