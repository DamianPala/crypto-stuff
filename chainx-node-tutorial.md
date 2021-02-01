# ChainX node setup tutorial

v.1.0

## Installation

1. Install rust: https://www.rust-lang.org/tools/install
2. Install wasm-pack: https://rustwasm.github.io/wasm-pack/installer/
3. Install screen and htop using `apt install`
4. Download chainx: https://github.com/chainx-org/ChainX/releases
5. Add access rights to the downloaded chainx: `chmod +x chainx...`
6. Copy downloaded chianx with renaming to `chainx`.
7. Type `crontab -e` and add a cron task to execute after the reboot: `@reboot sleep 3; /usr/bin/screen -d -m -S chainx /<your_chainx_path>/chainx --chain=mainnet --validator --name="your_node_name" --pruning=archive`.
  a) You can reattach to the screen session using `screen -r chainx`.
8. Reboot your machine and check if the chainx node is working using `htop`.

## Node registration

1. Go to dapps.chainx.org
2. Open `Stacking Overview` and click `Register a node` on the right top.
3. Setup name, initial mortgage e.g. 10 PCX only (the rest of the PCX you can delegate later using `Vote`)..
4. Go to the server and type: `curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:8086` to generate a session key
5. Go to the chainx app, click on `Developer` tab, select `Extrinsics`, select `session` on the left and choose `setKeys`.
  a) Copy result from the json to `keys` field.
  b) Type `0x00` in the `proof` field.
6.Submit transaction and confirm in the polka wallet.
7. Add identity - `Developer -> Extrinsics`, select `identity` and select `setIdentity`
  a) Set fields: `display`, `email` as required, the rest can be optional.

## Finalization

1. Check if the node is visible in chainx dapp on the Stacking Overview tab.
2. Delegate the rest of your PCX using `Vote` on your node.

by Hazo
