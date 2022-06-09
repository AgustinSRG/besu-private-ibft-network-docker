# Private IBFT network with Hyperledger besu + Docker

Hyperledger besu test network easy deployment with docker.

## Requeriments

 - [Hyperledger Besu](https://besu.hyperledger.org/en/stable/HowTo/Get-Started/Installation-Options/Options/)
 - [Docker](https://docs.docker.com/engine/install/)
 - [Docker compose](https://docs.docker.com/compose/install/)

## Configuration

Note: If you want just a test network, ignore this step and just use the default private keys.

This is a test network with 1 bootnode and 3 extra nodes.

Edit any parameters you need in the `config.json` file. Afer you are done, type the following command to generate the keys and the genesis block file:

```
besu operator generate-blockchain-config --config-file=config.json --to=networkFiles --private-key-file-name=key
```

The generator creates folders for each node you specified in the configuration. Rename the folders so you have: `bootnode`, `node1`, `node2` and `node3`.

After this, run a test enode to get the enode corresponding to the bootnode:

```
besu --data-path=data --genesis-file=./networkFiles/config/genesis.json --node-private-key-file=./networkFiles/keys/bootnode/key
```

Kill the node after you get the enode URL, and paste the ID in the `.env` file, as the value of `BOOTNODE_ID`.

Note: If you change the number of nodes, you must also edit `docker-compose.yml`

## Running

In order to run, type:

```
docker-compose up -d
```

In order to stop it, type:

```
docker-compose down
```

## Test it's working

Get list of validators:

```
curl -X POST --data "{\"jsonrpc\":\"2.0\",\"method\":\"ibft_getValidatorsByBlockNumber\",\"params\":[\"latest\"], \"id\":1}" http://localhost:8545
```

Get last block:

```
curl -X POST --data "{\"jsonrpc\":\"2.0\",\"method\":\"eth_getBlockByNumber\",\"params\":[\"latest\", false],\"id\":1}" http://localhost:8545
```
