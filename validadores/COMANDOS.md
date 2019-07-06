## configuración para el validador __(puedes encontrar estos tips en la [documentacion](https://kava.network/docs/validators/validator-setup.html#validator-setup) de kava y el cliente [Gaia](https://kava.network/docs/gaia/kvcli.html#gaia-cli))__:

## Comandos para `gaia-9002` & `Game of Stakes`:

* **Inicio de un nuevo nodo:**
```
kvd init --moniker <your_custom_moniker>
```

* **Inicio kvd:**
```
kvd start
```

* **Para la resolución de problemas:**
```
kvd start --trace --log_level "*:debug"
```

* **Comprobar el estado de kvd:**
```
kvcli status
```

* **Generar el `gentx` para `GoS`** (recuerda modificar los valores):
```
kvd gentx --amount 10000STAKE --commission-rate "0.10" --commission-max-rate "1.00" --commission-max-change-rate "0.01" --pubkey $(kvd tendermint show-validator) --name $(kvcli keys list | awk 'FNR==2{print $1}')
```

# Setup Validator:
* **Init validator:**
```
kvcli tx stake create-validator --amount=5STAKE --pubkey=$(kvd tendermint show-validator) --moniker="choose a moniker" --chain-id=<chain_id> --from=<key_name> --commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01"

```
* **Edit validator description:**
```
kvcli tx stake edit-validator --moniker="choose a moniker" --website="https://kava.io" --identity=6A2265E29A4CBC8E --details="To infinity and beyond!" --chain-id=<chain_id> --from=<key_name> --commission-rate="0.10"
```

* **View validator description:**
```
kvcli query stake validator <account_kava>
```

* **Generate Keys:**
```
kvcli keys add <account_name>
```

* **Show all address:**
```
kvcli keys list
```

* **Show address generate:**
```
kvcli keys show <account_name>
```

* **Validator operator's address:**
```
kvcli keys show <account_name> --bech=val
```

* **Validator pubkey for node:**
```
kvd tendermint show-validator
```

* **View validator description:**
```
kvcli query stake validator <account_kava>
```

* **Track validator signing information:**
```
kvcli query slashing signing-info <validator-pubkey> --chain-id=<chain_id>
```

* **Unjail validator:**
```
kvcli tx slashing unjail --from=$(kvcli keys list | awk 'FNR==2{print $1}') --chain-id=game_of_stakes_5 --trust-node=true
```

* **confirm validator is running:**
```
kvcli query tendermint-validator-set | grep "$(kvd tendermint show-validator)"
```

* **Account balance:**
```
kvcli query account <account_kava>
```

* **Send tokens:**
```
kvcli tx send --amount=10STAKE --chain-id=<chain_id> --from=<key_name> --to=<destination_kava>
```

* **Bond tokens:**
```
kvcli tx stake delegate --amount=10STAKE --validator=<validator> --from=<key_name> --chain-id=<chain_id>
```

* **See information about a validator:**
```
kvcli query stake delegation --address-delegator=<account_kava> --validator=<account_kavaval>
```

* **Check all current delegations with disctinct validators:**
```
kvcli query stake delegations <account_kava>
```

* **Unbond tokens:**
```
kvcli tx stake unbond --validator=<account_kavaval> --shares-fraction=0.5 --from=<key_name> --chain-id=<chain_id>
```

* **See information about `unbonding-delegation`:**
```
kvcli query stake unbonding-delegation --address-delegator=<account_kava> --validator=<account_kavaval>
```

* **Check all current `unbonding-delegations` with disctinct validators:**
```
kvcli query stake unbonding-delegations <account_kava>
```

* **Check all the `unbonding-delegations` from a particular validator:**
```
  kvcli query stake unbonding-delegations-from <account_kavaval>
```

* **Redelegate tokens:**
```
kvcli tx stake redelegate --addr-validator-source=<account_kavaval> --addr-validator-dest=<account_kavaval> --shares-fraction=50 --from=<key_name> --chain-id=<chain_id>
```

* **Query redelegations:**
```
kvcli query stake redelegation --address-delegator=<account_kava> --addr-validator-source=<account_kavaval> --addr-validator-dest=<account_kavaval>
```

* **Check all current `unbonding-delegations` with disctinct validators:**
```
kvcli query stake redelegations <account_kava>
```

* **All outgoing redelegations for a particula validator**
```
kvcli query stake redelegations-from <account_kavaval>
```

* **Current high level settings for staking:**
```
kvcli query stake parameters
```

# Governance:

* **Create a governance proposal:**
```
kvcli tx gov submit-proposal --title=<title> --description=<description> --type=<Text/ParameterChange/SoftwareUpgrade> --deposit=<40STAKE> --from=<name> --chain-id=<chain_id>
```

* **Query proposals (once created):**
```
kvcli query gov proposal --proposal-id=<proposal_id>
```

* **Query all available proposals:**
```
kvcli query gov proposals
```

* **Increased deposit proposals (default `10STAKE`):**
```
kvcli tx gov deposit --proposal-id=<proposal_id> --deposit=<200STAKE> --from=<name> --chain-id=<chain_id>
```

* **Query all deposits submited (once a new proposal is created):**
```
kvcli query gov deposits --proposal-id=<proposal_id>
```

* **Query deposit submitted (specific address):**
```
kvcli query gov deposit --proposal-id=<proposal_id> --depositor=<account_kava>
```

* **Vote on a proposal:**
```
kvcli tx gov vote --proposal-id=<proposal_id> --option=<Yes/No/NoWithVeto/Abstain> --from=<name> --chain-id=<chain_id>
```

* **Query votes (option submitted):**
```
kvcli query gov vote --proposal-id=<proposal_id> --voter=<account_kava>
```

* **Previous submitted proposal:**
```
kvcli query gov votes --proposal-id=<proposal_id>
```

* **Query proposal tally results:**
```
kvcli query gov tally --proposal-id=<proposal_id>
```



# Variables used:

* **Chain id:**
```
curl -s http://localhost:26657/status | jq -r '.result.node_info.network'
```

* **Account kava:**
```
kvcli keys list | awk 'FNR==2{print $3}'
```

* **Key name:**
```
kvcli keys list | awk 'FNR==2{print $1}'
```

* **kavavaloper:**
```
kvcli keys show $(kvcli keys list | awk 'FNR==2{print $1}') --bech=val | awk 'FNR==2{print $3}'
```

* **Balance:**
```
kvcli query account $(kvcli keys list | awk 'FNR==2{print $3}') --chain-id=${chain_id} --trust-node=true | jq -r '.value.coins[0].amount'
```
* **Proposal id:**
```
kvcli query gov proposals --trust-node=true | tail -n 1 | cut -d'-' -f1
```

* **Connect to Endpoints:**
```
ssh -i ~/.ssh/user_rsa -L 26657:localhost:26657 user@[ IP ]
```
