# RandomY Stratum Proxy

A high-performance Stratum proxy for RandomY-based cryptocurrencies, with automatic failover and **dev fee** support.

---

## Features

- **Multiple Pool Support:** Configure primary and failsafe mining pools for automatic fallback  
- **High Performance:** Optimized for minimal overhead and maximum hashrates  
- **Stable Connection:** Maintains stable connections with auto-reconnection to miners and pools  
- **Smart Job Distribution:** Efficiently distributes mining jobs to all connected miners  
- **Share Retry:** Automatically retries rejected shares with alternative nonce formats  
- **Comprehensive Statistics:** Real-time hashrate calculation and mining statistics  
- **Low Latency:** Minimizes share submission latency for higher efficiency  
- **Developer Fee:** Optional developer fee to support continued development  

---

## Installation

### Windows

1. Download the latest release from the [releases page](#).
2. Edit `config.json` with your pool and wallet information.

### Linux

```sh
chmod +x proxy
nano config.json
./proxy
```

Edit `config.json` with your pool and wallet information.

---

## Configuration

The proxy is configured using a `config.json` file:

```json
{
  "localPort": 3333,
  "localHost": "0.0.0.0",
  "poolHost": "corecoin.luckypool.io",
  "poolPort": 3118,
  "defaultWallet": "YOUR_WALLET_ADDRESS",
  "workerName": "Rig1",
  "devFeePercent": 2,
  "failsafePools": [
    { "host": "backup.pool1.com", "port": 3333, "name": "Backup 1" },
    { "host": "backup.pool2.com", "port": 3333, "name": "Backup 2" }
  ],
  "maxFailsafeReconnectAttempts": 3,
  "failsafeReconnectTimeout": 30000,
  "nonceLength": 16,
  "statsInterval": 300000,
  "debugMode": false,
  "quietMode": true,
  "logToFile": true,
  "logFile": "proxy-errors.log",
  "correctNonceFormat": true,
  "retryWithDifferentNonceFormat": true,
  "maxShareRetries": 2,
  "shareTransitionWindow": 8000
}
```

---

### Configuration Options

| Option                      | Description                                            | Default                 |
|-----------------------------|--------------------------------------------------------|-------------------------|
| `localPort`                 | Port on which the proxy will listen                    | 3333                    |
| `localHost`                 | IP address to bind to                                  | 0.0.0.0                 |
| `poolHost`                  | Primary mining pool address                            | corecoin.luckypool.io   |
| `poolPort`                  | Primary mining pool port                               | 3118                    |
| `defaultWallet`             | Your wallet address                                    | YOUR_WALLET_ADDRESS     |
| `workerName`                | Worker name to use for mining                          | Rig1                    |
| `devFeePercent`             | Percentage of time for dev fee mining                  | 2                       |
| `failsafePools`             | Backup pools to use if primary fails                   | See example             |
| `maxFailsafeReconnectAttempts` | Max reconnect attempts before switching pools        | 3                       |
| `failsafeReconnectTimeout`  | Delay between reconnection attempts (ms)               | 30000                   |
| `nonceLength`               | Length of nonce string                                 | 16                      |
| `statsInterval`             | Interval to print stats (ms)                           | 300000                  |
| `debugMode`                 | Enable detailed debug logging                          | false                   |
| `quietMode`                 | Suppress routine log messages                          | true                    |
| `logToFile`                 | Save error logs to file                                | true                    |
| `logFile`                   | Path to log file                                       | proxy-errors.log         |
| `correctNonceFormat`        | Fix nonce format to match pool requirements            | true                    |
| `retryWithDifferentNonceFormat` | Retry rejected shares with different formats       | true                    |
| `maxShareRetries`           | Max number of share retry attempts                     | 2                       |
| `shareTransitionWindow`     | Time window to accept shares from previous jobs (ms)   | 8000                    |
| `extranoncePosition` | How to position extranonce (`prepend`, `append`, or `none`) | `prepend` |
| `adaptiveExtranonceFormatting` | Automatically learn successful nonce formats | `true` |
| `ignoreWorkerWallets` | Overrides the workers wallet and yous the one from the config.json | `true` |
---

## Usage

Point your miners to the proxy.  
Configure your miners to connect to the proxy instead of directly to the pool:

```sh
MINING_SOFTWARE --server 127.0.0.1 --port 3333 --user YOUR_WALLET_ADDRESS.WORKER_NAME
```

Replace `MINING_SOFTWARE` with your mining software command (e.g., `xmrig`).

---

## Monitoring

The proxy displays statistics every few minutes (interval configurable):

```
=== RandomY Stratum Proxy Status ===
Time:         Uptime: 1h 32m
Pool:         [YOUR_WALLET.Rig1] - Connected & Logged in
Connected miners:   11
Shares:      1482 accepted / 3 rejected
Dev fee:     2% (91 shares, 2m 13s time)
Last accepted share: 3s ago
Total hashrate:     341.82 KH/s

Connected miners:
ID | Worker   | IP Address         | Shares (S/A/R)   | Hashrate    | Latency
---+----------+--------------------+------------------+-------------+---------
0  | Worker1  | 192.168.0.167:51746| 60/56/0          | 6.82 KH/s   | 72ms
1  | Worker2  | 192.168.0.185:55460| 240/201/0        | 38.83 KH/s  | 54ms
2  | Worker3  | 192.168.0.153:53292| 105/94/1         | 16.50 KH/s  | 54ms
...
```

---

## Special Thanks

Special thanks to all the miners supporting the development through the dev fee.  
Inspired by various open-source mining proxies and tools.

---

## Developer Fee

This proxy includes an **optional developer fee** (default: 2%) to support continued development.  
During the dev fee time, your miners will mine to the developer's address.  
The dev fee can be adjusted in the configuration, but a minimum of 2% is required for the software to function.

---

## Troubleshooting

**Q: Miners can't connect to the proxy**  
A: Ensure the `localPort` setting matches what your miners are trying to connect to, and that any firewalls allow the connection.

**Q: Proxy connects to the pool but shares are getting rejected**  
A: Try setting `correctNonceFormat` to `true` and enabling `retryWithDifferentNonceFormat` to automatically handle pool-specific share formats.

**Q: The proxy crashes after running for a while**  
A: Check the log file for errors. You may need to increase your system's maximum number of open files/connections.

**Q: Getting "Extranonce error" messages in the logs**  
A: Try changing the `extranoncePosition` setting to `append` or `none`. If the `adaptiveExtranonceFormatting` option is enabled, the proxy will attempt to learn the correct format automatically after a few errors.

---

## Support

For bugs, feature requests, or other questions:

- Open an issue on GitHub

---
