# GUILD-STABLECOIN-(GSC)
# Findings And Analysis Report

### 2024-06-23
### Table of Contents

- [Overview](#overview)
- [Summary](#summary)
- [Scope](#scope)
- [Severity Criteria](#severity)
- [High Risk Findings(0)](#high)
- [Medium Risk Findings(0)](#medium)
- [Low Risk and Non-critical Issues(1)](#low)
- [Gas Optimizations(1)](#gas)
- [Automated Findings](#auto)

## <font color="yellow">Overview <a id="overview"></a></font>
### About GSC
```Guild StableCoin (GSC)``` is a Crypto-collateralized stablecoin, backed by ```WETH``` and ```WBTC```, where the ```WETH``` and ```WBTC``` act as collateral and an *equivalent* amount of GSC get ```minted``` to the address of the  depositor of any/both tokens, as an exchange for her deposit(s). The amount of GSC gotten is calculated by the contract. usually the value of the collateral(in USD) must be greater than the stable coin(GSC) gotten.
The user can liquidate and get back her Collateral token depending on her ```health factor```.

## <font color="yellow">Summary<a id="summary"></a></font>
The Guild Stablecoin analysis yielded an aggregated total of 2 unique vulnerabilities. Of these vulnerabilities, 1 received a risk rating in the category of low/non-critical severity and 1 is a category of gas optimization.

## <font color="yellow">Scope<a id="scope"></a></font>
The code under review can be found within - [the cyfrin repository](https://github.com/Cyfrin/2023-07-foundry-defi-stablecoin)
, and is composed of 3 smart contracts written in the Solidity programming language and includes 236 lines of Solidity code.

## <font color="yellow">Gas optimization<a id="gas"></a></font>
## [G0-1] vulnerable code 
```
    constructor(address[] memory tokenAddresses, address[] memory priceFeedAddresses, address dscAddress) {
        // USD Price Feeds
        if (tokenAddresses.length != priceFeedAddresses.length) {
            revert DSCEngine__TokenAddressesAndPriceFeedAddressesMustBeSameLength();
        }
        // For example ETH / USD, BTC / USD, MKR / USD, etc
        for (uint256 i = 0; i < tokenAddresses.length; i++) {
            s_priceFeeds[tokenAddresses[i]] = priceFeedAddresses[i];
            s_collateralTokens.push(tokenAddresses[i]);
        }
        i_dsc = DecentralizedStableCoin(dscAddress);
    }
    ```
    instead of checking for the array length in every iteration, do the check before starting the ```for`` loop