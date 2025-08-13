# MEV Gas Fee Arbitrage Bot

A sophisticated Python-based MEV (Maximal Extractable Value) bot designed for gas fee arbitrage on the Optimism blockchain. The bot monitors the mempool in real-time to identify transactions with gas prices significantly above the base fee, then attempts to replace them with more efficiently priced transactions to capture the gas savings as profit.

## Features

- **Real-time Mempool Monitoring**: Connects to Optimism via Alchemy's WebSocket service
- **Gas Price Analysis**: Identifies transactions priced 20% above base fee
- **Automated Transaction Replacement**: Replaces with transactions at 5% above base fee
- **Profit Tracking**: SQLite database logs all profitable operations
- **Async Architecture**: Non-blocking concurrent operation
- **Comprehensive Logging**: Detailed transaction and profit analysis

## Requirements

- Python 3.8+
- Alchemy API key for Optimism mainnet
- Ethereum wallet with private key
- ETH balance for gas fees

## Installation

### For VS Code Development

1. **Clone or extract the project**
   ```bash
   cd mev-gas-arbitrage-bot
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r dependencies.txt
   ```
   
   Or install manually:
   ```bash
   pip install web3 python-dotenv eth-account websockets
   ```

4. **Create environment file**
   Create a `.env` file in the project root:
   ```
   ALCHEMY_RPC_URL=https://opt-mainnet.g.alchemy.com/v2/YOUR_API_KEY
   OP_URL=wss://opt-mainnet.g.alchemy.com/v2/YOUR_API_KEY
   WALLET_PRIVATE_KEY=0xYOUR_PRIVATE_KEY
   ```

5. **Run the bot**
   ```bash
   python mev_bot.py
   ```

## Configuration

### Environment Variables

- `ALCHEMY_RPC_URL`: HTTP RPC endpoint for Optimism mainnet
- `OP_URL`: WebSocket endpoint for real-time mempool monitoring  
- `WALLET_PRIVATE_KEY`: Private key for transaction signing

### Bot Parameters

You can modify these settings in `gas_analyzer.py`:

- `gas_threshold_multiplier`: Minimum gas price ratio to trigger replacement (default: 1.20 = 20%)
- `replacement_multiplier`: Gas price for replacement transactions (default: 1.05 = 5% above base fee)

## Project Structure

```
mev-gas-arbitrage-bot/
├── mev_bot.py              # Main bot orchestrator
├── gas_analyzer.py         # Gas price analysis logic
├── transaction_replacer.py # Transaction replacement handling
├── database.py             # SQLite database operations
├── dependencies.txt        # Python package requirements
├── setup.py               # Package installation configuration
├── README.md              # This documentation
└── .env                   # Environment variables (create this)
```

## Usage

### Starting the Bot

```bash
python mev_bot.py
```

The bot will:
1. Connect to Optimism mainnet via Alchemy
2. Initialize SQLite database for profit tracking
3. Start monitoring the mempool for pending transactions
4. Analyze gas prices and execute profitable replacements
5. Log all operations and display statistics every 5 minutes

### Monitoring Output

The bot provides detailed logging:
- Connection status and blockchain synchronization
- Real-time transaction analysis
- Profitable opportunity detection
- Replacement transaction execution
- Profit calculations and database logging

### Database Queries

View your profits using any SQLite browser or command line:

```sql
-- View all profitable replacements
SELECT * FROM profitable_replacements ORDER BY timestamp DESC;

-- Get total profits
SELECT SUM(estimated_savings_gwei) as total_gwei FROM profitable_replacements;

-- Count successful replacements
SELECT COUNT(*) as total_replacements FROM profitable_replacements;
```

## Security Considerations

- **Private Key Security**: Never commit your `.env` file to version control
- **Wallet Balance**: Ensure sufficient ETH for gas fees
- **Network Monitoring**: Monitor bot performance and blockchain connectivity
- **Address Validation**: Bot only replaces transactions from its own address

## Troubleshooting

### Common Issues

1. **"Missing required environment variables"**
   - Ensure `.env` file exists with all required variables
   - Check variable names match exactly

2. **"Failed to connect to Optimism"**
   - Verify Alchemy API key is valid
   - Check network connectivity

3. **"Account balance: 0.000000 ETH"**
   - Add ETH to your wallet for gas fees
   - Bot needs balance to send replacement transactions

4. **WebSocket connection errors**
   - Bot automatically reconnects on connection loss
   - Check Alchemy WebSocket endpoint URL

### Development Tips

- Use VS Code Python extension for debugging
- Set breakpoints in `process_transaction()` to analyze logic
- Monitor `mev_bot.log` file for detailed operation history
- Test with small amounts initially

## Performance Optimization

- **Transaction Filtering**: Add custom logic to filter transaction types
- **Gas Price Strategies**: Modify replacement pricing algorithms
- **Parallel Processing**: Increase concurrent transaction analysis
- **Database Optimization**: Add indexes for faster profit queries

## Disclaimer

This software is for educational and research purposes. MEV strategies involve financial risk. Always test thoroughly and understand the implications before deploying with real funds. The authors are not responsible for any financial losses.

## License

MIT License - see LICENSE file for details.