# RewardsController

The **RewardsController** is designed to store and distribute rewards to accounts that interact with the [Markets](market/)' different variable and fixed pools.

It calculates the total amount of rewards to distribute and determines the allocation between the pools based on a [dynamic distribution model](../features/rewards-distribution-model.md). Accounts can claim their rewards through the contract, and their claimable rewards can be queried at any time using the `claimable` function.

### Public State Variables

#### UTILIZATION\_CAP

```solidity
function UTILIZATION_CAP() external view returns (uint256)
```

Max utilization supported by the sigmoid function not to cause a division by zero.

#### distribution

```solidity
function distribution(contract Market) external view returns (uint8 availableRewardsCount, uint256 baseUnit)
```

Tracks the reward distribution data for a given market.

#### marketList

```solidity
function marketList(uint256) external view returns (contract Market)
```

Retrieves Markets with distributions set.

#### rewardEnabled

```solidity
function rewardEnabled(contract ERC20) external view returns (bool)
```

Tracks enabled asset rewards.

#### rewardList

```solidity
function rewardList(uint256) external view returns (contract ERC20)
```

Stores registered asset rewards.

### View Methods

#### accountOperation

```solidity
function accountOperation(address account, contract Market market, bool operation, contract ERC20 reward) external view returns (uint256, uint256)
```

Gets the account data of a given account, Market, operation and reward asset.

**Parameters**

| Name      | Type            | Description                                                    |
| --------- | --------------- | -------------------------------------------------------------- |
| account   | address         | The account to get the operation data from.                    |
| market    | contract Market | The market in which the operation was made.                    |
| operation | bool            | True if the operation was a borrow, false if it was a deposit. |
| reward    | contract ERC20  | The reward asset.                                              |

**Returns**

| Name | Type    | Description                 |
| ---- | ------- | --------------------------- |
| \_0  | uint256 | accrued The accrued amount. |
| \_1  | uint256 | index The account index.    |

#### allClaimable

```solidity
function allClaimable(address account, contract ERC20 reward) external view returns (uint256 unclaimedRewards)
```

Gets the claimable amount of rewards for a given account and reward asset.

**Parameters**

| Name    | Type           | Description                                  |
| ------- | -------------- | -------------------------------------------- |
| account | address        | The account to get the claimable amount for. |
| reward  | contract ERC20 | The reward asset.                            |

**Returns**

| Name             | Type    | Description                                 |
| ---------------- | ------- | ------------------------------------------- |
| unclaimedRewards | uint256 | The claimable amount for the given account. |

#### allMarketsOperations

```solidity
function allMarketsOperations() external view returns (struct RewardsController.MarketOperation[] marketOps)
```

Gets all market and operations.

**Returns**

| Name      | Type                                 | Description                    |
| --------- | ------------------------------------ | ------------------------------ |
| marketOps | RewardsController.MarketOperation\[] | The list of market operations. |

#### allRewards

```solidity
function allRewards() external view returns (contract ERC20[])
```

Retrieves all rewards addresses.

**Returns**

| Name | Type              | Description                  |
| ---- | ----------------- | ---------------------------- |
| \_0  | contract ERC20\[] | All enabled reward addresses |

#### availableRewardsCount

```solidity
function availableRewardsCount(contract Market market) external view returns (uint256)
```

Gets the amount of reward assets that are being distributed for a Market.

**Parameters**

| Name   | Type            | Description                                                  |
| ------ | --------------- | ------------------------------------------------------------ |
| market | contract Market | Market to get the number of available rewards to distribute. |

**Returns**

| Name | Type    | Description                               |
| ---- | ------- | ----------------------------------------- |
| \_0  | uint256 | The amount reward assets set to a Market. |

#### claimable

```solidity
function claimable(RewardsController.MarketOperation[] marketOps, address account, contract ERC20 reward) external view returns (uint256 unclaimedRewards)
```

Gets the claimable amount of rewards for a given account, Market operations and reward asset.

**Parameters**

| Name      | Type                                 | Description                                                                |
| --------- | ------------------------------------ | -------------------------------------------------------------------------- |
| marketOps | RewardsController.MarketOperation\[] | The list of Market operations to get the accrued and pending rewards from. |
| account   | address                              | The account to get the claimable amount for.                               |
| reward    | contract ERC20                       | The reward asset.                                                          |

**Returns**

| Name             | Type    | Description                                 |
| ---------------- | ------- | ------------------------------------------- |
| unclaimedRewards | uint256 | The claimable amount for the given account. |

#### distributionTime

```solidity
function distributionTime(contract Market market, contract ERC20 reward) external view returns (uint32, uint32, uint32)
```

Gets the distribution `start`, `end` and `lastUpdate` value of a given market and reward.

**Parameters**

| Name   | Type            | Description                               |
| ------ | --------------- | ----------------------------------------- |
| market | contract Market | The market to get the distribution times. |
| reward | contract ERC20  | The reward asset.                         |

**Returns**

| Name | Type   | Description                                            |
| ---- | ------ | ------------------------------------------------------ |
| \_0  | uint32 | The distribution `start`, `end` and `lastUpdate` time. |

#### previewAllocation

```solidity
function previewAllocation(contract Market market, contract ERC20 reward, uint256 deltaTime) external view returns (uint256 borrowIndex, uint256 depositIndex, uint256 newUndistributed)
```

Retrieves projected distribution indexes and new undistributed amount for a given `deltaTime`.

**Parameters**

| Name      | Type            | Description                                    |
| --------- | --------------- | ---------------------------------------------- |
| market    | contract Market | The market to calculate the indexes for.       |
| reward    | contract ERC20  | The reward asset to calculate the indexes for. |
| deltaTime | uint256         | The elapsed time since the last update.        |

**Returns**

| Name             | Type    | Description                                        |
| ---------------- | ------- | -------------------------------------------------- |
| borrowIndex      | uint256 | The index for the borrow operation.                |
| depositIndex     | uint256 | The index for the deposit operation.               |
| newUndistributed | uint256 | The new undistributed rewards of the distribution. |

#### rewardConfig

```solidity
function rewardConfig(contract Market market, contract ERC20 reward) external view returns (struct RewardsController.Config)
```

Gets the configuration of a given distribution.

**Parameters**

| Name   | Type            | Description                                           |
| ------ | --------------- | ----------------------------------------------------- |
| market | contract Market | The market to get the distribution configuration for. |
| reward | contract ERC20  | The reward asset.                                     |

**Returns**

| Name | Type                     | Description                     |
| ---- | ------------------------ | ------------------------------- |
| \_0  | RewardsController.Config | The distribution configuration. |

#### rewardIndexes

```solidity
function rewardIndexes(contract Market market, contract ERC20 reward) external view returns (uint256, uint256, uint256)
```

Gets the reward indexes and last amount of undistributed rewards for a given market and reward asset.

**Parameters**

| Name   | Type            | Description                                     |
| ------ | --------------- | ----------------------------------------------- |
| market | contract Market | The market to get the reward indexes for.       |
| reward | contract ERC20  | The reward asset to get the reward indexes for. |

**Returns**

| Name | Type    | Description                                                        |
| ---- | ------- | ------------------------------------------------------------------ |
| \_0  | uint256 | borrowIndex The index for the floating and fixed borrow operation. |
| \_1  | uint256 | depositIndex The index for the floating deposit operation.         |
| \_2  | uint256 | lastUndistributed The last amount of undistributed rewards.        |

### Write Methods

#### claim

```solidity
function claim(RewardsController.MarketOperation[] marketOps, address to, contract ERC20[] rewardsList) external nonpayable returns (contract ERC20[], uint256[] claimedAmounts)
```

Claims `msg.sender` rewards for the given operations and reward assets to the given account.

**Parameters**

| Name        | Type                                 | Description                          |
| ----------- | ------------------------------------ | ------------------------------------ |
| marketOps   | RewardsController.MarketOperation\[] | The operations to claim rewards for. |
| to          | address                              | The address to send the rewards to.  |
| rewardsList | contract ERC20\[]                    | The list of rewards assets to claim. |

**Returns**

| Name           | Type              | Description                  |
| -------------- | ----------------- | ---------------------------- |
| \_0            | contract ERC20\[] | The list of rewards assets.  |
| claimedAmounts | uint256\[]        | The list of claimed amounts. |

#### claimAll

```solidity
function claimAll(address to) external nonpayable returns (contract ERC20[] rewardsList, uint256[] claimedAmounts)
```

Claims all `msg.sender` rewards to the given account.

**Parameters**

| Name | Type    | Description                         |
| ---- | ------- | ----------------------------------- |
| to   | address | The address to send the rewards to. |

**Returns**

| Name           | Type              | Description                  |
| -------------- | ----------------- | ---------------------------- |
| rewardsList    | contract ERC20\[] | The list of rewards assets.  |
| claimedAmounts | uint256\[]        | The list of claimed amounts. |

#### config

```solidity
function config(RewardsController.Config[] configs) external nonpayable
```

Enables or updates the reward distribution for the given markets and rewards.

**Parameters**

| Name    | Type                        | Description                                        |
| ------- | --------------------------- | -------------------------------------------------- |
| configs | RewardsController.Config\[] | The configurations to update each RewardData with. |

#### handleBorrow

```solidity
function handleBorrow(address account) external nonpayable
```

Hook to be called by the Market to update the index of the account that made a rewarded borrow.

**Parameters**

| Name    | Type    | Description                                |
| ------- | ------- | ------------------------------------------ |
| account | address | The account to which the index is updated. |

#### handleDeposit

```solidity
function handleDeposit(address account) external nonpayable
```

Hook to be called by the Market to update the index of the account that made a rewarded deposit.

**Parameters**

| Name    | Type    | Description                                |
| ------- | ------- | ------------------------------------------ |
| account | address | The account to which the index is updated. |

#### withdraw

```solidity
function withdraw(contract ERC20 asset, address to) external nonpayable
```

Withdraws the contract's balance of the given asset to the given address. Only to be called by ADMIN role accounts.

**Parameters**

| Name  | Type           | Description                           |
| ----- | -------------- | ------------------------------------- |
| asset | contract ERC20 | The asset to withdraw.                |
| to    | address        | The address to withdraw the asset to. |
