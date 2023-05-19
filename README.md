# Get All Arbitrum UniswapV2 Pools
## Script to get all pools from UniswapV2-compatible DEX's by querying Arbitrum blockchain data directly

**Description:**
This is a hack of a Python script I previously wrote - leveraging eth-brownie - to query all UniswapV2 compatible factories and pools to build and output a set of viable LPs and associated data.

Some LPs are void - maybe they are empty / have little reserves or give another error - and some ERC20s are also void.  I try to at least weed through the initial set of noise.  I have other (private) scripts that work with this data and do a better job of eliminating worthless LPs and ERC20s.

### (dev tasks)
1. Setup brownie and its config to handle Arbitrum chain
    * Maybe also alternative chains and more than just one DEX
2. Make appropriate Uniswap interfaces and import them into the script (done)
3. Start calling and printing and see what comes of it (done)

### General procedural flow:

I may describe my code here later.

### Development notes:
* Removed Stargate from the set of DEXs because, even though their factory used to be compatible with UniswapV2, their LPs definitely are not and their factory recently resulted in reversions.
* Arbitrum Exchange has the following code for determining if deposit plus fees >= K 
''' 
 { // scope for reserve{0,1}Adjusted, avoids stack too deep errors
            uint _swapFee = swapFee;
            uint balance0Adjusted = (balance0.mul(10000).sub(amount0In.mul(_swapFee)));
            uint balance1Adjusted = (balance1.mul(10000).sub(amount1In.mul(_swapFee)));
            require(balance0Adjusted.mul(balance1Adjusted) >= uint(_reserve0).mul(_reserve1).mul(10000**2), 'ArbDex K');
        }
'''
    The 'mul(10000**2)' may be an issue.  Though it seems like this works out with both balances being adjusted.
* Added Auragi (another Solidly fork) -- https://docs.auragi.finance/auragi-finance/
* Added MIND Games DEX, a small V2 fork -- https://docs.mindgames.io/about/contracts
* Added MagicSwap because the pools seem inbalanced (even though it's only got 3 pools) - https://docs.treasure.lol/references/contracts
* Added Swapr, which has some pools with > $2000 liquidity - https://swapr.gitbook.io/swapr/contracts
* Added Swapfish, which has even fewer pools with > $2000 in liquidity - https://docs.swapfish.fi/security/contracts