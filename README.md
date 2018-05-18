Simple setup to test basics of parity's [block reward contracts](https://github.com/paritytech/parity/pull/8419)

## How-to

1. install parity 1.11 (tested on 1.11.0)
2. clone repository
```bash
git clone https://github.com/phahulin/test-block-reward.git
cd test-block-reward
```
3. install dependencies
```bash
npm install
```
4. start two parity nodes for two validators in separate terminal windows
```bash
parity --config node1.toml
```
and
```bash
parity --config node2.toml
```
5. wait till you see both `enode://...` printed to logs, then tell nodes about each other:
```bash
$ ./mate.sh 
true
true
```
you should see 1/25 peers appear in each log

6. start script to track balances
```
node catch-all.js
```

7. to reset the network and remove chain files, run
```
./clear-all.sh
```

## Expected result
Up until block 20 you should see validators receive 1 eth reward to their mining key in round-robin fashion. From block 20 onwards they will start receiving 10 eth reward to payout key + 5 eth will be sent to vault address.

Note that this contract [cannot emit events](https://github.com/paritytech/parity/pull/8419#issuecomment-389303455)

## Test Data
* [BlockReward contract source](./contracts/TestBlockReward.sol)
* [spec.json](./spec.json) notice `blockRewardContractAddress` and `blockRewardContractTransition` parameters. Bytecode is stored in corresponding `account` section
* Validator keys:
    - mining `0x6546ed725e88fa728a908f9ee9d61f50edc40ad6`, payout `0x7546ed725e88fa728a908f9ee9d61f50edc40ad6`
    - mining `0x1a22d96792666863f429a85623e6d4ca173d26ab`, payout `0x2a22d96792666863f429a85623e6d4ca173d26ab`
* Vault address: `0xe9d0bb7fa991960cf9bcff4899e8fec3b25e77f2`
