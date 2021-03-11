# ChainX node setup tutorial

v.1.0

## Installation

1. Setup environment:   
```
curl https://getsubstrate.io -sSf | bash -s -- --fast
source ~/.cargo/env
```
2. Install screen and htop using `apt install`
3. Download chainx: https://github.com/chainx-org/ChainX/releases
	1. You can use for instance `wget https://github.com/chainx-org/ChainX/releases/download/v2.0.8-1/chainx-v2.0.8-1-ubuntu-20.04-x86_64-unknown-linux-gnu` remember, you should always download latest release.
4. Add access rights to the downloaded chainx: `chmod +x chainx...`
5. Copy downloaded ChianX with renaming to `chainx`.
6. Prepare configuration file using the following template and name it `config`:
```
{
  "log-dir": "./log", // Log directory
  "enable-console-log": false, // At the same time output the log to the console
  "no-mdns": true,
  "validator": true, // Must be true for validator node
  "ws-external": false, // Recommended to close for validators
  "rpc-external": false, // Recommended to close for validators
  "log": "info,runtime=info",
  "port": 20222, // Specify the tcp port of the p2p protocol
  "ws-port": 8087, // Specify the RPC service port of websocket
  "rpc-port": 8086, // Specify http RPC service port
  "pruning": "archive", // It is strongly recommended to add this configuration and start in archive mode
  "execution": "NativeElseWasm",
  "db-cache": 2048, // Set the cache of the node database, the unit is MB, which is 2GB here
  "state-cache-size": 2147483648, // Set the node state tree cache, unit B, which is 2GB here (2GB = 2 * 1024 * 1024)
  "name": "your_node_name", // The node name displayed in the node browser Telemetry
  "base-path": "your_database_path", // The database path, where the chain data will be stored
  "bootnodes": []
}
```

  Please remove comments from this template before use.

7. Type `crontab -e` and add a cron task to execute after the reboot: `@reboot sleep 3; /usr/bin/screen -d -m -S chainx /<your_chainx_path>/chainx --chain=mainnet --config /<your_chainx_path>/config`.
	1. You can reattach to the screen session using `screen -r chainx`.
8. Reboot your machine and check if the chainx node is working using `htop`.

## Node registration

1. Go to dapps.chainx.org
2. Open `Stacking Overview` and click `Register a node` on the right top.
3. Setup name, initial mortgage e.g. 10 PCX only (the rest of the PCX you can delegate later using `Vote`)..
4. Go to the server and type: `curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:8086` to generate a session key
5. Go to the ChainX Dapp, click on `Developer` tab, select `Extrinsics`, select `session` on the left and choose `setKeys`.
	1. Copy result from the json to `keys` field.
	2. Type `0x00` in the `proof` field.
6.Submit transaction and confirm in the polka wallet.
7. Add identity - `Developer -> Extrinsics`, select `identity` and select `setIdentity`
	1. Set fields: `display`, `email` as required, the rest can be optional.

## Finalization

1. Check if the node is visible in chainx dapp on the Stacking Overview tab.
2. Delegate the rest of your PCX using `Vote` on your node.

## Migration to new server

In case when you want to migrate your node from one server to another, you need to be very careful. There are penalty for node offline time, name change and more.

1. Setup the new node on the new server
    1. Use the same node name as your old node
2. Wait until your new node will be fully synchronized. You can check this in the [ChainX Telemetry](https://telemetry.polkadot.io/#list/ChainX)
3. Generate new session key using: `curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:8086`
4. Load new session key in the ChainX Dapp
5. Disable old node.
6. Check if you are getting the PCX reward in the Dapp.

# Useful stuff

1. DApp: https://dapps.chainx.org/
2. Documentation (use Google Translator to translate from Chinese)): https://chainx-org.github.io/documentation/zh/docs/
3. Telemetry: https://telemetry.polkadot.io/#list/ChainX

by Hazo