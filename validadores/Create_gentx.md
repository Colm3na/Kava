## Steps to create `gentx`:

# If we need to delete address:
```
kvcli keys delete $(kvcli keys list | awk 'FNR==2{print $1}') 
```

# Reset values:
```
kvd unsafe-reset-all 
```

# Backup of your old files (remember, somes names of files can be diferent):
```
mv .kvd/ /home/user/old.copies/
```

# Recover address:
```
kvcli keys add --recover <ADDRESSNAME>
```

# Run `kvd init`:
```
kvd init --moniker=$(kvcli keys list | awk 'FNR==2{print $1}') 
```

# Download genesis:
```
wget https://raw.githubusercontent.com/Kava-Labs/kava/tree/master/testnet-1.1/genesis.json > $HOME/.kvd/config/genesis.json 
```

# Generate `gentx`:
```
kvd gentx --amount 1000000000000ukva --commission-rate "0.10" --commission-max-rate "1.00" --commission-max-change-rate "0.01" --pubkey $(kvd tendermint show-validator) --name $(kvcli keys list | awk 'FNR==2{print $1}') 
```
