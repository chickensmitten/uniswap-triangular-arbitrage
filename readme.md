# Uniswap Triangular Arbitrage Javascript
## Intro
- Refer to this for more information [poloniex triangular arbitrage](https://github.com/chickensmitten/poloniex-triangular-arbitrage)

- EtherJS is easier to convert big numbers and easier to interact with ABI
- Refer to Lesson 82 onwards for JS implementation with EtherJS

## Operation
- This code base searches for depth in the triangle arbitrage. However, it needs the JSON file out put from [Uniswap Python codebase](https://github.com/chickensmitten/uniswap-triangular-arbitrage-python). Refer to code `let fileInfo = getFile("../Uniswap/uniswap_surface_rates.json");`
  - JSON file has the following format
  ```
  {
    "swap1":"WETH",
    "swap2":"USDC",
    "swap3":"ADS",
    "poolContract1":"0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640",
    "poolContract2":"0xc0b51c7042be877bedeee2103f3f6667e32bee97",
    "poolContract3":"0xb9044f46dcdea7ecebbd918a9659ba8239bd9f37",
    "poolDirectionTrade1":"quoteToBase",
    "poolDirectionTrade2":"baseToQuote",
    "poolDirectionTrade3":"quoteToBase",
    "startingAmount":1,
    "acquiredCoinT1":1704.0208988544275,
    "acquiredCoinT2":1469.1941764835817,
    "acquiredCoinT3":1.9397143550135487,
    "swap1Rate":1704.0208988544275,
    "swap2Rate":0.862192580778373,
    "swap3Rate":0.001320257312519524,
    "profitLoss":0.9397143550135487,
    "profitLossPerc":93.97143550135488,
    "direction":"reverse",
    "tradeDesc1":"Start with WETH of 1. Swap at 1704.0208988544275 for USDC acquiring 1704.0208988544275.",
    "tradeDesc2":"Swap 1704.0208988544275 of USDC at 0.862192580778373 for ADS acquiring 1469.1941764835817.",
    "tradeDesc3":"Swap 1469.1941764835817 of ADS at 0.001320257312519524 for WETH acquiring 1.9397143550135487."
  }
  ```
- [Uniswap Python codebase](https://github.com/chickensmitten/uniswap-triangular-arbitrage-python) is required to get the price, format prices, calculate surface rate arbitrage.
  -  To get the prices, refer to the code below, `quoteExactInputSingle` is an ABI available in the Uniswap address, It requires the token address to be added in, the token address for the token to be taken out, along with the fee and also the amount for the token in.
  ```
  const quoterAddress = "0xb27308f9F90D607463bb33eA1BeBb41C27CE5AB6";
  const quoterContract = new ethers.Contract(quoterAddress, QuoterABI, provider)
  let quotedAmountOut = 0
  quotedAmountOut = await quoterContract.callStatic.quoteExactInputSingle(
    inputTokenA,
    inputTokenB,
    tokenFee,
    amountIn,
    0
  )
  ```
- The arbitrage depth in uniswap is also known as slippage and is very much dependent on the liquidity in the pool.
- run `node main` to execute `main.js` file.