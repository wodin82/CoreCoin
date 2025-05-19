RandomY Stratum Proxy
A high-performance stratum proxy for RandomY-based cryptocurrencies with automatic failover and dev fee support.
Show Image
Show Image
Features

Multiple Pool Support: Configure primary and failsafe mining pools for automatic fallback
High Performance: Optimized for minimal overhead and maximum hashrates
Stable Connection: Maintains stable connections with auto-reconnection to miners and pools
Smart Job Distribution: Efficiently distributes mining jobs to all connected miners
Share Retry: Automatically retries rejected shares with alternative nonce formats
Comprehensive Statistics: Real-time hashrate calculation and mining statistics
Low Latency: Minimizes share submission latency for higher efficiency
Developer Fee: Optional developer fee to support continued development

Installation
Windows

Download the latest release from the releases page
Edit config.json with your pool and wallet information

Linux

# Make the binary executable
chmod +x proxy

# Edit the configuration
nano config.json

# Run the proxy
./proxy
Configuration
The proxy is configured using a config.json file:
json{
    "localPort": <LOCAL PORT>,
    "localHost": "0.0.0.0",
    "poolHost": "<POOL>",
    "poolPort": <PORT>,
    "defaultWallet": "YOUR_WALLET_ADDRESS",
    "workerName": "Rig1",
    "devFeePercent": 2,
    "failsafePools": [
        {
            "host": "<POOL>",
            "port": <PORT>,
            "name": "<NAME>"
        },
        {
            "host": "<POOL>",
            "port": <PORT>,
            "name": "<NAME>"
        }
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
Configuration Options
OptionDescriptionDefaultlocalPortPort on which the proxy will listen3333localHostIP address to bind to0.0.0.0poolHostPrimary mining pool addresscorecoin.luckypool.iopoolPortPrimary mining pool port3118defaultWalletYour wallet addressYOUR_WALLET_ADDRESSworkerNameWorker name to use for miningRig1devFeePercentPercentage of time for dev fee mining2failsafePoolsArray of backup pools to use if primary failsSee examplemaxFailsafeReconnectAttemptsMax attempts to reconnect to a pool before trying next3failsafeReconnectTimeoutDelay between reconnection attempts (ms)30000nonceLengthLength of nonce string16statsIntervalInterval to print stats (ms)300000debugModeEnable detailed debug loggingfalsequietModeSuppress routine log messagestruelogToFileSave error logs to filetruelogFilePath to log fileproxy-errors.logcorrectNonceFormatFix nonce format to match pool requirementstrueretryWithDifferentNonceFormatRetry rejected shares with different formatstruemaxShareRetriesMaximum number of share retry attempts2shareTransitionWindowTime window to accept shares from previous jobs (ms)8000
Usage
Point your miners to the proxy
Configure your miners to connect to the proxy instead of directly to the pool:
MINING_SOFTWARE --server 127.0.0.1 --port 3333 --user YOUR_WALLET_ADDRESS.WORKER_NAME
Replace MINING_SOFTWARE with your mining software command (e.g., xmrig).
Monitor the proxy
The proxy displays statistics every few minutes (configurable):
=== RandomY Stratum Proxy Status ===
Time: <TIME>
Uptime: 1h 32m
Pool: <POOL> [YOUR_WALLET.Rig1] - Connected & Logged in
Connected miners: 11
Shares: 1482 accepted / 3 rejected
Dev fee: 2% (91 shares, 2m 13s time)
Last accepted share: 3s ago
Total hashrate: 341.82 KH/s

Connected miners:
ID    | Worker     | IP Address        | Shares (S/A/R) | Hashrate     | Latency
------+------------+-------------------+----------------+-------------+---------
0     | Worker1    | 192.168.0.167:51746 | 60/56/0 | 6.82 KH/s   | 72ms
1     | Worker 2   | 192.168.0.185:55460 | 240/201/0 | 38.83 KH/s  | 54ms
2     | Worker 3   | 192.168.0.153:53292 | 105/94/1 | 16.50 KH/s  | 54ms
...
Special thanks to all the miners supporting the development through the dev fee
Inspired by various open-source mining proxies and tools

Developer Fee
This proxy includes an optional developer fee (default: 2%) to support continued development. During the dev fee time, your miners will mine to the developer's address. The dev fee can be adjusted in the configuration, but a minimum of 2% is required for the software to function.
Troubleshooting
Common Issues
Q: Miners can't connect to the proxy
A: Ensure the localPort setting matches what your miners are trying to connect to, and that any firewalls allow the connection.
Q: Proxy connects to the pool but shares are getting rejected
A: Try setting correctNonceFormat to true and enabling retryWithDifferentNonceFormat to automatically handle pool-specific share formats.
Q: The proxy crashes after running for a while
A: Check the log file for errors. You may need to increase your system's maximum number of open files/connections.
Support
For bugs, feature requests, or other questions:

Open an issue on GitHub
