# KoraX (KORX) — Independent Proof-of-Work Blockchain

**KoraX (KORX)** is an independent Layer-1 blockchain derived from Bitcoin Core, using the **SHA-256d Proof-of-Work** consensus algorithm with its own Genesis block, deterministic monetary rules, and native Bech32 addresses (`korax1...`).

---

## 1. Technical & Consensus Specifications
- **Name / Ticker**: KoraX (`KORX`)
- **Consensus**: SHA-256d Proof-of-Work
- **Maximum Supply (`MAX_MONEY`)**: `20,000,000 KORX` (hard-capped in `src/consensus/amount.h`)
- **Creator Allocation (Block 1)**: `5,000,000 KORX` (verified by consensus at height 1)
- **Mineable Supply**: `15,000,000 KORX`
- **Initial Block Reward**: `50 KORX` per block (starting from Block 2)
- **Halving Interval**: Every `210,000 blocks`
- **Target Block Time**: `10 minutes` (600 seconds)
- **Difficulty Retargeting**: Every `2016 blocks`
- **Address Format**: Bech32 (`korax1...`) | Base58 Prefix: `45`
- **Default P2P Port**: `38333` | **Magic Bytes**: `0x4B 0x4F 0x52 0x58` (`KORX`)
- **Genesis Block Hash**: `00009694264733aaa603999e1013c735a166e05bfb43b356ac498c6b7efab0ab`
- **Official Explorer & Website**: https://vercel.app

---

## 2. Node Installation & Mining Guide (Linux / PC / Termux)

### Build from Source
```bash
git clone https://github.com
cd KoraX
cmake -B build && cmake --build build -j\$(nproc)
```

### Secure Configuration (`korax.conf`)
Never expose RPC port `28332` to the public Internet:
```ini
chain=korax
server=1
listen=1
discover=1
port=38333
rpcport=28332
bind=0.0.0.0:38333
rpcbind=127.0.0.1
rpcallowip=127.0.0.1
fallbackfee=0.0001
```

### Start Node & Mine Blocks
```bash
./build/bin/bitcoind -chain=korax -daemon
./build/bin/bitcoin-cli -chain=korax createwallet "miner"
ADDR=\$(./build/bin/bitcoin-cli -chain=korax -rpcwallet=miner getnewaddress "" "bech32")
./build/bin/bitcoin-cli -chain=korax generatetoaddress 1 "\$ADDR"
```
*(Coinbase mining rewards require 100 block confirmations before becoming spendable.)*

---

## 3. Wallet Management & Security
- **Create & Encrypt**:
  ```bash
  ./build/bin/bitcoin-cli -chain=korax createwallet "my_wallet"
  ./build/bin/bitcoin-cli -chain=korax -rpcwallet=my_wallet encryptwallet "STRONG_PASSWORD"
  ```
- **Backup & Restore**:
  ```bash
  ./build/bin/bitcoin-cli -chain=korax -rpcwallet=my_wallet backupwallet "/safe/path/korax_backup.dat"
  ./build/bin/bitcoin-cli -chain=korax restorewallet "restored_wallet" "/safe/path/korax_backup.dat"
  ```

---

## 4. Security Status & Transparency
- **Verified by Unit Tests (`korax_block_tests`)**: Hard cap at `20,000,000 KORX`, exact `5,000,000 KORX` Block 1 creator allocation, `50 KORX` reward from Block 2, and halving at `210,000` blocks.
- **Current Limitations**: No external third-party security audit yet; initial network difficulty (`0x1f00ffff`) is low while community hashpower grows.
