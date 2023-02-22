# Sorted array using Circom

## Install dependencies

To install all the dependencies run:

```
yarn install
```

## Compile circuits and generate and verify the zk-proof using [snarkjs](https://github.com/iden3/snarkjs)

To learn workflow when working with circom circuits check [Circom docs](https://docs.circom.io/getting-started/compiling-circuits/)

```
cd .\circuits\
circom array.circom --r1cs --wasm --sym --c
node ./array_js/generate_witness.js ./array_js/array.wasm input.json ./array_js/witness.wtns
cd .\array_js\
snarkjs powersoftau new bn128 12 pot12_0000.ptau -v
snarkjs powersoftau contribute pot12_0000.ptau pot12_0001.ptau --name="First contribution" -v
snarkjs powersoftau prepare phase2 pot12_0001.ptau pot12_final.ptau -v
snarkjs groth16 setup ./../array.r1cs pot12_final.ptau array_0000.zkey
snarkjs zkey contribute array_0000.zkey array_0001.zkey --name="1st Contributor Name" -v
snarkjs zkey export verificationkey array_0001.zkey verification_key.json
snarkjs groth16 prove array_0001.zkey witness.wtns proof.json public.json
snarkjs groth16 verify verification_key.json public.json proof.json

```

## Run tests

```bash
yarn test
```
