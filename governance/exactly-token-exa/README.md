# ⚫ Exactly Token (EXA)

**`EXA` Smart Contract Address (OP Mainnet):**&#x20;

[**`0x1e925de1c68ef83bd98ee3e130ef14a50309c01b`**](https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b)

The EXA token grants holders the right to vote on system changes and upgrades. Holders of the EXA token will wield power over the Protocol’s [treasury](https://docs.exact.ly/guides/parameters#b.-treasury-fee) and [smart contract upgrades](https://docs.exact.ly/security/access-control).

As the Protocol develops, EXA token holders will play a crucial role in decision-making, including setting collateral requirements, introducing new collateral types, adjusting borrowing and lending terms, and managing token incentives. These decisions demand thoughtful evaluation of various factors, and the Exactly team will provide research, analysis, and open-source simulations to assist the community in making informed decisions.

### **Timelock**&#x20;

* For security reasons, most of the EXA tokens will be deposited in the [Timelock Contract](https://optimistic.etherscan.io/address/0x92024C4bDa9DA602b711B9AbB610d072018eb58b):\
  [https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b?a=0x92024c4bda9da602b711b9abb610d072018eb58b](https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b?a=0x92024c4bda9da602b711b9abb610d072018eb58b)

### Circulating Supply & Token Holders

* **EXA token holders and Total Supply:** [https://optimistic.etherscan.io/token/tokenholderchart/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b](https://optimistic.etherscan.io/token/tokenholderchart/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b)
* **Exa token Circulating Supply**: [https://app.exact.ly/api/circulating-exa](https://app.exact.ly/api/circulating-exa)
*   **Circulating Supply Formula:**

    ```
    circulatingSupply = totalSupply - nonCirculatingSupply + totalWithdrawable
    ```

&#x20;       **nonCirculatingSupply** => sum of the EXA balance of:\
&#x20;       [`Sablier_V2_Lockup_Dynamic`](https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b?a=0x6f68516c21e248cddfaf4898e66b2b0adee0e0d6)\
&#x20;       [`SablierV2LockupLinear`](https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b?a=0xb923abdca17aed90eb5ec5e407bd37164f632bfd)\
&#x20;       [`TimelockController`](https://optimistic.etherscan.io/address/0x92024C4bDa9DA602b711B9AbB610d072018eb58b)\
&#x20;       [`RewardsController`](https://optimistic.etherscan.io/address/0xBd1ba78A3976cAB420A9203E6ef14D18C2B2E031)\
&#x20;       [`EscrowedEXA`](https://optimistic.etherscan.io/address/0xbea586A167853ADddEF12818f264f1F9823fBc18)\
&#x20;       [`Treasury`](https://optimistic.etherscan.io/address/0x23fd464e0b0ee21cedeb929b19cabf9bd5215019)\
&#x20;       [`Airdrop`](https://optimistic.etherscan.io/token/0x1e925de1c68ef83bd98ee3e130ef14a50309c01b?a=0x3cecea7ef91b6f6d3760f6b5845c3332dc00a420)

&#x20;        **totalWithdrawable** => the sum of the amount withdrawable from each Sablier stream

### EXA token Unlock Schedule

* **DefiLlama**: [https://defillama.com/unlocks/exactly](https://defillama.com/unlocks/exactly)

### Dune Dashboards

* **The EXA Token**: [https://dune.com/exactly/exa](https://dune.com/exactly/exa)
* **The EXA Community Airdrop**: [https://dune.com/exactly/exactly-airdrop](https://dune.com/exactly/exactly-airdrop)

### Price Info

* **CoinGecko:** [https://www.coingecko.com/en/coins/exactly-token](https://www.coingecko.com/en/coins/exactly-token)
* **CoinMarketCap:** [https://coinmarketcap.com/currencies/exactly-protocol/](https://coinmarketcap.com/currencies/exactly-protocol/)
* **DeFiLlama:** [https://defillama.com/protocol/exactly?tokenPrice=true](https://defillama.com/protocol/exactly?tokenPrice=true)
* **GeckoTerminal:** [https://www.geckoterminal.com/optimism/pools/0xf3c45b45223df6071a478851b9c17e0630fdf535](https://www.geckoterminal.com/optimism/pools/0xf3c45b45223df6071a478851b9c17e0630fdf535)

### Velodrome DEX

* **Velodrome EXA/WETH Liquidity Pool:** \
  [https://velodrome.finance/swap?from=0x4200000000000000000000000000000000000006\&to=0x1e925de1c68ef83bd98ee3e130ef14a50309c01b](https://velodrome.finance/swap?from=0x4200000000000000000000000000000000000006\&to=0x1e925de1c68ef83bd98ee3e130ef14a50309c01b)

### Coinstore Exchange

* **EXA/USDT spot:** [https://www.coinstore.com/spot/EXAUSDT](https://www.coinstore.com/spot/EXAUSDT)

### EXA/WETH Vaults

* **EXA/WETH LP Vault on Beefy:** \
  [https://app.beefy.com/vault/velodrome-v2-exa-weth](https://app.beefy.com/vault/velodrome-v2-exa-weth)
*   **Leveraged yield farming pool on Extra Finance:**&#x20;

    [https://app.extrafi.io/farm/vAMMV2-EXA%2FWETH](https://app.extrafi.io/farm/vAMMV2-EXA%2FWETH)
* **EXA/WETH Vault on Yearn Finance:** [https://yearn.finance/vaults/10/0xc3439Ba7db7566ed0deF55c179ED9b3bA273A67F](https://yearn.finance/vaults/10/0xc3439Ba7db7566ed0deF55c179ED9b3bA273A67F)

### Deposit EXA

* **Extra Finance:** [https://app.extrafi.io/lend/EXA](https://app.extrafi.io/lend/EXA)

### Get EXA

* **Short URL**: [https://app.exact.ly/get-exa](https://app.exact.ly/get-exa)

