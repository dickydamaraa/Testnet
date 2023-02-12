<p style="font-size:14px" align="right">
<a href="https://t.me/dickydamara" target="_blank">Contact me on Telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
<a href="https://discordapp.com/users/392347017818669056" target="_blank">Contact me on Discord <img src="https://user-images.githubusercontent.com/50621007/176236430-53b0f4de-41ff-41f7-92a1-4233890a90c8.png" width="30"/></a>
<a href="https://twitter.com/snollyygoster" target="_blank">Contact me on twitter <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Twitter-logo.svg/2491px-Twitter-logo.svg.png" width="30"/></a>
<a href="https://snollygoster.xyz/" target="_blank">Visit my website <img src="https://raw.githubusercontent.com/dickydamaraa/explorer-1/master/public/logox.png" width="30"/></a>
</p>


<p align="center">
  <img height="100" height="auto" src="https://cdn.builder.io/api/v1/image/assets%2F580ff9284d33405f94bd899116dbdf56%2F1846b26b1cf2456bb5da6004e6629645?width=132">
</p>


# Alliance node setup testnet
Official documentation:
- Faucet : https://game-of-alliance.terra.money/faucet
- Explorer: -
- Github: https://github.com/terra-money/alliance/
- Discord: https://discord.gg/terra-money
- Docs alliance terra : https://alliance.terra.money/

## Chain Alliance Terra
• Chain ID: ordos-1 - Denom: uord - Executable: ordosd
• Chain ID: corrino-1 - Denom: ucor - Executable: corrinod
• Chain ID: harkonnen-1 - Denom: uhar - Executable: harkonnend
• Chain ID: atreides-1 - Denom: uatr - Executable: atreidesd

## Hardware requirements
Same with any cosmos-SDK chain, check this out!

Minimum Hardware Requirements : \
• Ubuntu 22.04 \
• 4 CPU \ 
• 8gb ram \
• 200GB SSD NVME \ 
• Permanent Internet connection (traffic will be minimal during run node; 10Mbps will be plenty - for production at least 100Mbps is expected) \
• Open port 26656

**Thats requirements if you want run with local device, but my advice it will great if you rent a VPS and your server will online 24x7**

## Node setup
### • Automatic Installation •
You can setup your mainnet node in few minutes with this script below. Just follow the instructions.\
Note : **I prefer on manual, so you can learn new experience to solve problem :) **
```
wget -O jointest.sh https://raw.githubusercontent.com/Testnet/main/Alliance-Tera/auto.sh && chmod +x jointest.sh
```

### Execute installation with chain
```
./jointest.sh -c {CHAIN}  # CHANGE {CHAIN} AS YOU WANT
```
Example :
```
./jointest.sh -c ordos-1
```
============================================================================================
## Step after installation
After you install binaries, you need to read again on here, what executable you should use. 
-
Example :
If you install with chain harkonnen, you need use executable `harkonennend` 
{EXECUTABLE} > Change with chain with you install before 
{CHAIN ID}   > Change with same chain before
{DENOM}      > Change with same denom with chain



### Create wallet
To create new wallet
```
{EXECUTABLE} keys add wallet
```
Change `wallet` to name own your wallet

To recover wallet existing keys with mneomenic 
```
{EXECUTABLE} keys add wallet --recover
```
Change `wallet` to name own your wallet

To see current keys 
```
{EXECUTABLE} keys list
```

### Create validator
Make sure your node was synced to false status, before create validator

To check if your node is synced simply run
`{EXECUTABLE} status 2>&1 | jq .SyncInfo`

Creating validator with `5000000 {DENOM}`

```
{EXECUTABLE} tx staking create-validator \
    --amount=5000000 {DENOM} \
    --pubkey=$({EXECUTABLE} tendermint show-validator) \
    --moniker={MONIKER} \
    --chain-id={CHAIN-ID} \
    --from=node \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1"
```

## Usefull commands
### Service management
Check logs
```
journalctl -fu {EXECUTABLE} -o cat
```

Start service
```
sudo systemctl start {EXECUTABLE}
```

Stop service
```
sudo systemctl stop {EXECUTABLE}
```

Restart service
```
sudo systemctl restart {EXECUTABLE}
```

### Node info
Synchronization info
```
{EXECUTABLE} status 2>&1 | jq .SyncInfo
```

Validator info
```
{EXECUTABLE} status 2>&1 | jq .ValidatorInfo
```

Node info
```
{EXECUTABLE} status 2>&1 | jq .NodeInfo
```

Show node id
```
{EXECUTABLE} tendermint show-node-id
```

### Wallet operations
List all wallets
```
{EXECUTABLE} keys list
```

Recover your wallet with phrase
```
{EXECUTABLE} keys add wallet --recover
```

Delete your wallet
```
{EXECUTABLE} keys delete wallet
```

Check your wallet balance
```
{EXECUTABLE} query bank balances <address>
```

### Staking, Delegation and Rewards
Delegate stake
```
{EXECUTABLE} tx staking delegate <valoper address> <amount>{DENOM} --from=wallet --chain-id={CHAIN_ID} --gas=auto
```

Redelegate stake from validator to another validator
```
{EXECUTABLE} tx staking redelegate <srcValidatorAddress> <destValidatorAddress> <amount>{DENOM} --from=wallet --chain-id={CHAIN_ID} --gas=auto
```

Withdraw all rewards
```
{EXECUTABLE} tx distribution withdraw-all-rewards --from=wallet --chain-id={CHAIN_ID} --gas=auto
```

Withdraw rewards with commision
```
{EXECUTABLE} tx distribution withdraw-rewards <valoper address> --from=wallet --chain-id={CHAIN_ID} --gas=auto
```

### Validator management
Edit validator
```
{EXECUTABLE} tx staking edit-validator \
  --moniker=$MONIKER \
```
Unjail validator
```
{EXECUTABLE} tx slashing unjail \
  --broadcast-mode=block \
  --from=wallet \
  --chain-id={CHAIN-ID} \
  --gas=auto
```

### Delete node
```
sudo systemctl stop {EXECUTABLE} && \
sudo systemctl disable {EXECUTABLE} && \
rm /etc/systemd/system/{EXECUTABLE}.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .{EXECUTABLE} && \
rm -rf $(which {EXECUTABLE})
```
