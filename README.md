## ZK Circuilt Implementation
To design a new zkSNARK circuit that implements some logical operations. We need to implement the circuit and deploy a verifier on-chain to verify proofs generated from this circuit.
## Description
For the Final Challenge, The project should contain:

1. Write a correct circuit.circom implementation
2. Compile the circuit to generate circuit intermediaries
3. Generate a proof using inputs A=0 B=1
4. Deploy a solidity verifier to Sepolia or Mumbai Testnet
5. Call the verifyProof() method on the verifier contract and assert output is true

## Getting Started
### Executing program
To run this program, We can use online Gitpod Platform. You can visit the Gitpod website at https://www.gitpod.io/
1. Install the dependencies using :- npm i
2. To compile the code and generate MultiplierVerifier.sol run the command :- npx hardhat circom
3. To deploy the code on gitpod :- npx hardhat run scripts/deploy.ts
   The script does 4 things
   1. Deploys the MultiplierVerifier.sol contract
   2. Generates a proof from circuit intermediaries with generateProof()
   3. Generates calldata with generateCallData()
   4. Calls verifyProof() on the verifier contract with calldata
4. Finally connect it to the sepolia network using the command :- npx hardhat run scripts/deploy.ts --network sepolia
5. Check the verification on etherscan sepolia testnet and write the contract address generate to check and verify using the link https://sepolia.etherscan.io/

```
pragma circom 2.0.0;


template CustomCircuit () {

    // signal inputs
    signal input a;
    signal input b;

    // signal from gates
    signal x;
    signal y;

    // signal output 
    signal output q;

    // component used to create Custom Circuit
    component andGate = AND();
    component notGate = NOT();
    component orGate = OR();

    // Circuit Logic
        // And Gate
    andGate.a <== a;
    andGate.b <== b;
    x <== andGate.out;

        // Not Gate  
    notGate.in <== b;
    y <== notGate.out;

        // OR Gate
    orGate.a <== x;
    orGate.b <== y;

    q <== orGate.out;

}

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}
template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;
}

component main = CustomCircuit();

```
6. The circuit uses three component templates: AND, NOT, and NOR Gates. These are reusable building blocks of the circuit, implemented as separate templates.
7. CustomCircuit() creates a CIRCOM component named main, which corresponds to the entire circuit defined in the CustomCircuit template.


## License

This project is licensed under the MIT License - see the LICENSE.md file for details
