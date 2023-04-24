# 🧪 Testing

The smart contract tests can be accessed at [Exactly's Github repository](https://github.com/exactly/protocol/tree/main/test). These tests are categorized into two types: Typescript tests utilizing [Hardhat's development environment](https://github.com/NomicFoundation/hardhat), and Solidity tests leveraging [Foundry's testing framework, Forge](https://github.com/foundry-rs/foundry/tree/master/forge).

Our team continuously updates, adds, and reviews these tests. Anyone is welcome to try them independently. To do so, clone the project, install the necessary dependencies, and execute the command `yarn test`.

In addition, Exactly employs [Forge's Fuzzer](https://book.getfoundry.sh/forge/fuzz-testing) as another tool for smart contract testing. Fuzzing is an essential testing method, as it can reveal edge cases that are difficult to identify through manual unit testing. The fuzz tests for Exactly Protocol can be found in the [Protocol.t.sol](https://github.com/exactly/protocol/blob/main/test/solidity/Protocol.t.sol) file, which also serves as the protocol's specifications.

### Coverage

Exactly's project is integrated with the [Codecov app](https://about.codecov.io/), allowing users to view real-time code coverage by visiting [Exactly's page](https://app.codecov.io/gh/exactly/protocol).
