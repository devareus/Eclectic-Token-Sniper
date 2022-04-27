# Eclectic-Token-Sniper
Eclectic Token Sniper (ETSniper) is a versatile token sniper for the blockchain. It should work with any decentralized exchange compatible with the Uniswap V2 API on any blockchain compatible with Ethereum. For example, just to mention a few:

#### Chains
- Ethereum
- BNB Smart Chain (former Binance Smart Chain)
- Avalanche C-Chain
- Polygon
- Fantom Opera
- Harmony ONE

#### DEXs
- UniswapV2
- SushiSwap
- PancakeSwap
- ApeSwap
- Biswap
- BakerySwap
- Traderjoexyz

The Honeypot Check functionality is available for the Binance Smart Chain, Polygon, Fantom and Avalanche (including their mainnets and testnets).

The Safe-buy functionality is available for the Binance Smart Chain, Polygon and Fantom (mainnets and testnets).

I can add support for others on request. Send me (0xa9f5B4Fd93ddA4eb247CC1cC726a6602a2Ce7F00) enough native tokens of the corresponding blockchain so I can deploy the contracts and get in touch!

## Main features

- Support for multiple chains, decentralised exchanges and liquidity pairs
- Able to monitor, buy and sell multiple tokens from different sources simultaneously
- Multiple methods for sniping tokens:
	- From a list of token addresses to watch
	- From PairCreated events
	- From AddLiquidityETH transactions
	- From Telegram channels/groups, including private ones and obfuscated addresses
	- Mempool snipping of AddLiquidity transactions
- Tokens auditing, including:
	- Very quick and efficient in-house and in-chain honeypot check, that checks if a token can be approved and sold, with configurable limits for buy and sell taxes and gas usage
	- Contract source verification and filtering by a configurable list of expressions
	- Minimum and maximum liquidity amount and minimum percentage
- Optional buy using a safety contract that checks for honeypot, liquidity and drawdown within the same buy transaction and reverts it back if the token doesn't fulfil the requirements
- Whitelisting of token addresses
- Blacklisting of addresses, symbols and names
- Stops buying tokens when the account balance is below a configurable threshold
- Monitoring of bought tokens that supports:
	- Profit estimation, including estimation of sell tax and gas
	- Taking profit after reaching a certain value
	- Setting a fixed stop loss or based on the estimated drawdown
	- Setting a trailing stop
	- Disabling the sell of a token for a certain time after being bought
	- Selling a token after a certain amount of time after buying
	- Selling a token if the trailing stop doesn't change after a certain amount of time 
	- Mempool monitoring to frontrun RemoveLiquidity transactions
- Simulation mode to test your sources and settings without risking any real funds
***

For bug reports, feedback or help, use the Issues functionality here on Github, send an email to [devareus@protonmail.com](devareus@protonmail.com) or join the
Telegram channel at [http://t.me/EclecticTokenSniper](http://t.me/EclecticTokenSniper).
 
A 7.5% developer fee is paid on all profits above 0.01 base tokens (BNB in the BSC, MATIC on Polygon, etc). The profit is calculated as: *amount sold* - *amount bought* - *all gas costs*.

Buying using the safe-buy contract has a fee of 1.5% of the amount paid if successful. Nothing is charged otherwise. 

If you want to support further development of this sniper you can send a donation on Ethereum or BSC to: 

0xa9f5B4Fd93ddA4eb247CC1cC726a6602a2Ce7F00


## How to run it
Download the corresponding executable for your platform from the Releases section and place it in an empty folder. At the moment there are binaries available for Windows x64, Linux x64, Linux ARM (32 and 64 bits) and MacOS. If you need binaries for another platform, get in touch and I'll try to build one.

Download the appsettings-template.json file, copy it as appsettings.json and put in the same folder as the executable.
Edit it following the instructions below.

Double click on the executable, or better, open a terminal/console and run it from it.

You can quit the sniper by pressing Ctrl+C. The status of the sniper is saved each time a token is bought, sold or the trailing stops are changed, so if you launch it again from the same folder, it will carry on monitoring them.

If you want to run the sniper on different chains, exchanges or LiquidityPairAddress, it's recommended to run each from a different folder.

## appsettings.json (v220427)

NOTE: The JSON standard doesn't allow comments, but the library I use does, so I used them to add clarity to the appsettings template. Feel free to remove them if they bother you.

### Accounts
  - WalletPrivateKey: Your account private key. It's a string of numbers and letters that you can export, from example, from your Metamask. *It's not the list of recovery words that gives access to all the accounts in your Metamask, but a separate key for each account in your wallet*. It's recommended to use a separate account so the sniper doesn't interfere with your normal activity or is affected by it. Don't hold in this account more funds than you need
  - BscScanApiKey: If you want to check if a token's contract is verified, you'll need to get an API key from BSCScan and put it here

### Node
  - NodeAddressHttp: The RPC endpoint of the blockchain node that you'll connect to. You can use one of the official nodes, get a node from moralis.io or deploy your own node
  - NodeAddressWss: For some snipping options you'll also need a websocket node endpoint. You can use the nariox node, get one from moralis.io or use your own one
  - GetContractSourceUrl: The URL for downloading the tokens' contracts' source codes from a blockchain explorer.

- NativeTokenSymbol: Symbol of the native token of the chain (BNB, MATIC, FTM, AVAX, Metis, ONE, etc). You can generally leave it blank and the bot will find it out automatically. But with some non-standard DEXs you might need to set it manually.

### DEX
  - ExchangeRouterAddress: The contract address for the exchange's router. For example, for PancakeSwap it's "0x10ed43c718714eb63d5aa57b78b54704e256024e"
  - LiquidityPairAddress: The token that will be paired for liquidity in the DEX. If empty ("") the native token of the blockchain (BNB, for example) will be used directly to buy the tokens (if there's enough liquidity in the pair). It it's set to the contract address of a token, the native token will be swapped for this intermediary token before buying the snipped token. For example, if set to "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56" on the BSC (BUSD), the *AmountToSnipe* amount of BNB will be converted to BUSD and these used to buy the snipped token.
  
### Global settings
  - TransactionRevertTimeSeconds: Maximum time for a transaction to be confirmed before it's automatically cancelled
  - MonitorPricesEverySeconds: When 0, it disables the monitoring of the prices of the tokens the sniper has bought. If set to a number, the sniper will check the prices regularly at the set interval (in seconds)
  - WatchTokensInterval: Indicates the interval, in seconds, at which the tokens in the watched list will be checked for sniping. Each time the interval elapses a new token from the list will be checked, starting again for the beginning once the list is exhausted

### Simulation mode

  - SimulationMode: If enabled, the bot will run in simulation mode, without submitting any real transactions. This can be useful to see if your sources or settings can make consistent profits, without having to risk your real money while you adjust them. Mind that some things, like how other transactions in the current block will impact prices, can't be simulated so the simulated results could differ significantly from what would have happened in reality. Simulations will be more accurate on blockchains where the HoneypotCheck contract is deployed
  - SimulationModeWalletBalance: Initial simulated balance the the first time you run the bot in simulation mode or after deleting the *simulation-balance.json* file
  
  **You don't need the whole SimulationModeWalletBalance amount in your real wallet, but even though it won't be spent, you need at least enough for the AmountToSnipe + gas. Otherwise all they buy simulations will fail.**

  - SimulationModeBuyDelay: Simulated time for your buy transactions to be processed. The token price used for your simulated transaction will depend on this. It's recommended to set it to the block time of the chain or around that
  - SimulationModeApproveDelay: Same as the above, but for approve transactions
  - SimulationModeSellDelay: Similarly, for sell transactions. Will determine what token price will be used to simulate your sells

### Snipers
  - WatchedTokensSniper: Enables or disables the watch list. See [Watch List](#watch-list) section below
- TelegramSniper: If enabled, the sniper will listed on the configured Telegram channels, filtering by keywords and sniping token addresses for it. Use only with reliable sources
 - PairCreatedEventSniper: If enabled, the sniper will listen for PairCreated events for new tokens to snipe
  - AddLiquiditySniper: If enabled, the sniper will listen for all AddLiquidity transactions on the DEX for tokens to snipe. This method is not recommended unless you limit buys to the white list, or you'll end buying lots of old tokens
  - MempoolSniper: If enabled, the sniper will monitor the mempool for AddLiquidity transactions to try to buy in the same block or the following adjusting the buy gas price accordingly
- PairCreatedEventSniperParametersSet: ParametersSet to use for the PairCreated events sniper
- AddLiquiditySniperParametersSet: ParametersSet to use for the AddLiquidity sniper
- MempoolSniperParametersSet: ParameterSet to use for the Mempool sniper
- MempoolSniperMaxGasPrice: Maximum gas price when buying from the mempool sniper
- MempoolSniperGasPriceAdjustment: By default the bot uses the same gas price as the transaction found in the mempool. It's not recommended to increase it because as you risk front running the transaction and the buy will fail because the liquidity addition isn't effective yet. And if you set a negative number (lower the gas price) other bots will most surely buy before you.

### Buy settings
-    BuyEnabled: Lets you disable the buy of tokens globally. This can be toggled while running by pressing the B key, but it will revert back to the default configured here if the sniper is restarted
-    MinWalletBalance: Minimum balance in your account for buying tokens. The sniper won't buy any more tokens if your balance is below this figure. If the balance increases (for example, you send more funds, or a token is sold) it will start buying again. I recommend to set this to slightly more than the *AmountToSnipe*, to guarantee that some funds are left to pay for the networks fees when selling.
-    OnlyBuyWhitelisted: Will only buy tokens included in the white list

### Sell settings
-    SellEnabled: Lets you disable the sell the tokens globally. This can be toggled while running by pressing the S key, but it will revert back to the default configured here if the sniper is restarted
-    MempoolRemoveLiquidityEmergencySell: If enabled, the mempool is monitored for RemoveLiquidity transactions of the owned tokens. If one is detected the bot will try to frontrun the removal transaction to sell the tokens before the liquidity is gone. Mind that any RemoveLiquidity of the token will trigger this, even if it's only partial, so false positives are possible
-    EmergencySellGasPriceAdjustment: Adjustment of gas price for emergency approvals and sells, in relation to the gas price of the RemoveLiquidity transaction that triggered them. Should be at least 1 to have any chances of frontrunning it. The higher the value the better chances of selling before the liquidity is gone, but mind the costs!
-    EmergencySellMaxGasPrice: Maximum gas price for emergency approvals and sells. This is a safe net in case a RemoveLiquidity had a crazy gas price
-    EmergencySellSlippage: Maximum slippage for emergency sells. You probably want a bigger percentage than usual here, as it's better to recover some of your investment than nothing.

### White/Black Listing
-    WhitelistedTokens: This list of token addresses can have two different but similar usages. If *OnlyBuyWhitelisted* is enabled, only the tokens in this list can be bought. If it's disabled, it skips any audits for these tokens (use with care!)
-    BlacklistedTokens: A list of token addresses to never buy
-    BlacklistedTokenSymbols: Tokens whose symbols contain these strings won't be bought. This can be use to filter out tokens with symbols that contain, for example, "doge", "elon", "musk", "shiba", etc, as these are usually scams, or "test" which usually are... well... that... tests :)
-    BlacklistedTokenNames: Same as above, but for the token names
-    ContractRedFlagStrings: If contract verification is enabled, this is the list of expressions that prevent buying a token if any of them is present in its source code

### Telegram Sniper
-    TelegramApiId: Telegram Account Id for the Telegram sniping method
-    TelegramApiHash: Telegram Account Hash for the Telegram sniping method
-    TelegramApiPhoneNo: Telegram Phone Number for the Telegram sniping method

*To obtain your Telegram API credentials follow the instructions here: [https://core.telegram.org/api/obtaining_api_id](https://core.telegram.org/api/obtaining_api_id). **Use at your own risk as whether Telegram allows this kind of use is a bit of a grey area and they could ban you forever from their service***

-  TelegramSilenceChatsList: Set to true if you don't want to see the list of Telegram channels or groups you're connected to, each time that you start the bot
 -   TelegramListenerConfigurations: This a list of rules for sniping tokens from Telegram channels. For each rule you can set:
		- RuleName: A name for this rule, for your reference
		- ChatId: The *username* (the part after *t.me/* URLs, for public ones) or the numerical ID (for private ones) of the channel/group to listen to for this rule. You must already be a member of the channel/group. You can obtain these from the list shown when you start the Telegram sniper
		- AdminsOnly: Enable it to only get addresses from messages of the admins of the chat
		- IgnoreBots: Usually necessary if using the previous option, as bots are normally admins and you don't want to buy a token when a bot mentions a user that has a token address in his name
		- SolveMathExpressions: Enable to solve math expressions in the messages when searching for contract addresses. For example, 0xae13d(4+5)89daC2f0dEbFf460aC112a837C89BAa7cd would be solved into 0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd
		- IgnoreBlankSpace: Enable to ignore all blank space when searching for contract addresses. This helps with addresses split across several lines
		- RequiredText: All these words or expressions need to be present in a message for it to be parsed in search of a token address
		- ExcludeText: If any of these words or expressions is present in a message, it will be ignored and not used for snipping
		- ParametersSet: The set of parameters for this rule (see the [Parameters Sets](#parameters-sets) section above). The default one will be used if none is specified
		- AddToWatchList (optional): If *true* the token address will be added to the watch list if the buy fails when the announcement is received. This is useful if a token is announced before is available for trading
		- StopWatchListCheckingAfterMinutes (optional): If *AddToWatchList* is enabled, this setting allows you to set a number of minutes after which the token will be removed from the watch list if it not bought yet

### Additional Settings

- LogParametersSets: If enabled computes and dumps the values of all the ParametersSets
- CheckSeen: If enabled, it will check again a token that has been previously bought or discarded, if the sniper finds it again. It's recommended to leave it disabled
- BuySeen: If enabled, it allows buying a token again, even if it's been previously bought or discarded, if the sniper finds it again. It's recommended to leave it disabled
- DeadWallets: List of dead wallets used for the calculation of the percentage of the total supply as liquidity. It's recommended not to touch this

You can ignore the rest of the file, it's mainly log files configuration.

## Parameters Sets

This section allows you to define sets of parameters to be used for the different snipers, watched tokens or Telegram rules. At the very least there needs to be a *"default"* set (it needs to be literally named *"default"*), which will be used by the snipers unless another set is specified. All the parameters need to be set for the default one. The other sets can consist of just a few settings if so desired, in which case the *"default"* values will be used for those missing.

The available settings are:

### Gas settings
  - BuyGasPrice: Gas price of buy transactions
  - ApproveGasPrice: Gas price for approval transactions
  - SellGasPrice: Gas price for sell transactions
  - BuyGasAmount: Maximum gas amount for buy transactions
  - ApproveGasAmount: Maximum gas amount for approval transactions
  - SellGasAmount: Maximum gas amount for sell transactions
  - EstimateGasOnBuy: If enabled, an estimation of gas is done before submitting buy transactions and checked against the maximum amount above. This has the advantage of not wasting gas in aborted transactions if these are going to fail due to being out-of-gas, but the disadvantage of making the snip operation slower. You can leave it off if using the HoneypotCheck functionality, as the gas limits are already checked by it
  - EstimateGasOnSell: Similar to the above, but for sell transactions
   
### Buy settings
-    BuyEnabled: Enables or disables the buy of tokens for the sniper, rule or watched token using this set
-    AmountToSnipe: The default amount to buy of each snipped token (in the native token of the chain, BNB for example in the case of the BSC) 
-    AuditTokens: Enables the audit of tokens before buying. You can enable or disable the followings checks separately:
    
        -    HoneypotCheckEnabled: Using an in-house and in-chain solution checks if a token can be bought, approved and sold before buying. It can also check for maximum taxes, gas and drawdowns, using the limits set below. This option requires HoneypotCheck support for the blockchain and exchange. At the moment BSC and Polygon are supported. If you need others, send us a request
        -    CheckContract: Only buys if the token's contract is verified on BSCScan and it doesn't contain any of the expressions in the red flags list
        -    CheckLiquidity: Doesn't buy if the available liquidity doesn't fulfil the limits below

**Mind that the tokens owners can change the taxes at any time! That a token passes an audit is not guarantee that you'll be able to sell the token later or you won't be charged higher taxes. And obviously, it doesn't guarantee that the owner won't do a rug pull and remove all the liquidity from the DEX at any some point**

- MaxBuyTax: Used for the token audits. Maximum Buy Tax for sniping a token
- MaxSellTax: Similarly, maximum Sell Tax for sniping a token
- MaxTotalTax: Maximum total of taxes (buy+sell) for snipping a token
- MaxDrawdown: The estimated *drawdown* is the estimated loss caused by the price impacts, taxes and gas fees if the token was bought and sold immediately. The bought won't buy a token if the estimated drawdown is higher than this
-    MinLiquidityAmount: Minimum liquidity in BNB (or the LiquidityPair if one has been specified) in the DEX to allow buying a token (if *CheckLiquidity* is enabled)
-    MaxLiquidityAmount: Maximum liquidity in BNB (or the LiquidityPair if one has been specified) in the DEX to allow buying a token (if *CheckLiquidity* is enabled). Set to -1 for unlimited
-    MinLiquidityPercentage: Minimum liquidity, as a percentage of the total supply, in the DEX to allow buying a token (if *CheckLiquidity* is enabled)
-    BuyDelaySeconds: Waits the indicated amount of seconds before buying a token. This is usually used to avoid antibots measures, but the Honeypot Check method is more reliable
- UseSafeBuyContract: Buy using the safe-buy contract. If enabled, a special contract will be used to buy that will check for honeypot, liquidity and tax&price impact limits in the same transaction, reverting it if it's not safe to buy the token. 

**Using this contract has an additional dev fee of 1.5% of the amount invested plus a higher cost in gas, as the contract will have to check liquidity and simulate a buy, an approval and a sell, before the effective buy, and all these tests will be done by the miner processing the transaction and not just a plain node like in the normal honeypot check (which doesn't consume gas), and the gas usage can be relatively high when buying a token with a complex transfer function.**

**For failed buys, depending on which step the checks fail you'll pay more or less gas: once a condition isn't met, the contract doesn't test the others so as to save some gas.**

**Unless you're snipping from the mempool or you really want to be as fast as possible in submitting a buy while still doing some checks you'll be better off by just using the plain honeypot check, which is slower but free**

The following settings let you fine tune the safe-buy feature:

- SafeBuyCheckHoneypot: Simulates to buy, approve and sell before actually buying
- SafeBuyCheckHoneypot: Doesn't buy if the liquidity is lower than *MinLiquidityAmount* above
- SafeBuyCheckMaxLiquidity: Doesn't buy if the liquidity is higher than *MaxLiquidityAmount* above
- SafeBuyCheckMaxTaxAndPriceImpact: Doesn't buy if the estimated loss due to taxes and price impact is higher than the parameter below.
- SafeBuyMaxTaxAndPriceImpact: Maximum estimated loss due to taxes and price impact. It's similar to the *MaxDrawdown* setting above, but doesn't consider the gas costs, as you're going to pay most of them anyway by running the buy&sell simulation within the buy transaction and calculating them would only increase the gas costs of using the contract for buying with very little gas saving if reverting the transaction because of it.

### Sell settings

-    SellEnabled: Enables or disables selling the tokens
-    TakeProfit: Sells a token if the current profit is above this figure. It's indicated as a percentage above the buy price, for example, 50 means "sell when the profit is above 50%", that is, the current price is about 150% of the buy price
-    StopLoss: Sets a stop loss. A token will be sold if its price goes below this percentage in relation to the buy price. For example, -25 means "sell when the price has gone down by a 25%"
- StopLossBasedOnDrawdown: Calculate the initial stop loss from the estimated drawdown (see an explanation of drawdown above). This option requires HoneypotCheck support for the blockchain and exchange. At the moment BSC and Polygon are supported. If you need others, send us a request
- StopLossBasedOnDrawdownMargin: How much below the estimated drawdown to set initial stop loss if the previous option is enabled. For example, if the estimated drawdown of a token just bought is 17.75% and the margin is 12, the initial stop loss would be set to -27.75%
-    TrailingStopStrategy: If enabled and the price of a token increases above a threshold, the stop loss will be adjusted at a certain distance from the current price. It can be set to:

		- none: To disable it
		- buy: the threshold and distance will be calculated as a percentage of the buy price
		- current: the thresholds and distance will be calculated as a percentage of the current price
		- profit: the thresholds and distance will be calculated as a percentage of the current profit
-    TrailingStopActivationThreshold: How much the price has to increase above the buy price for the stop loss to be changed
-    TrailingStopThreshold: How much the price has to increase above the previous stop loss for this to be changed
-    TrailingStopDistance: Distance at which the new stop loss will be set
-    SellSlippage: Maximum slippage set during selling
-    DontSellBeforeMinutes: Prevents selling a token, even if the stop loss is touched, for the first few minutes after buying a token. The trailing stop (if enabled) won't be active during this time either. This can be used to prevent being taken out of the market by the initial volatility after a token is launched. Use at your own risk!
-    SellAfterMinutes: Sells tokens after the amount of minutes indicated in this setting, no matter if any of the other conditions above is fulfilled or not. Set to -1 to disable
- SellIfTrailingStopNotChangedInMinutes: Sells tokens if the trailing stop hasn't changed after the amount of minutes indicated in this setting, no matter if any of the other conditions above is fulfilled or not. Set to -1 to disable
- ApplySellGasTaxToProfitEstimations: If enabled, an estimation of sell tax and approval and sell gas is deducted from the profit estimation used for the take profit, stop loss and trailing stop. This option requires HoneypotCheck support for the blockchain and exchange. At the moment BSC and Polygon are supported. If you need others, send us a request
- RecheckSellGasTaxMinutes: Sets the frequency for updating the tax and gas estimations used for the above
- ApprovalMethod: Defines when the approval to sell the tokens will be done. It can be set to one of the following:

	- preapprove: Sends the approve request just after the token is bought, except if the account had the token already approved
	- check: At sell time, checks if the token is already approved and sends the transaction if it isn't
	- always: At sell time, it just sends the approval request straight away so as to not waste any time in doing any checks

For (mempool) emergency sells the *always* method will be used, unless the token has been pre-approved.

- 	WaitForApproval: If enabled, waits for the confirmation of the approve transaction before submitting the sell transaction. It's recommended to leave it disabled, unless for some estrange reason you're having frequent issues with approvals. For emergency sells approvals are never waited for, even if this option is set, as the point is to try to frontrun the liquidity removal

**Each bought token remembers the name of the set used when it was bought, even after restarting the bot; be careful if you change the sell settings of a parameters set for which there are still bought tokens being monitored as this will affect them. If you don't want to affect the sell parameters of already owned tokens, create and use a new set for new buys**

## Watch List
Can be enabled with *WatchedTokensSniper*. The sniper will audit once and again the tokens in the *watched-tokens.json* file and will try to buy them once they pass the audit. This, with *AuditTokens* and *HoneypotCheckEnabled* enabled, is probably the most reliable method to snip known token addresses.

The list of watched tokens is stored in a file named *watched-tokens.json* (you can find a template in the release files). Multiple entries are supported, with each one having the following attributes:

  - Address: The contract address of the token to watch and snipe
  - ParametersSet (optional): The set of parameters for this rule (see the [Parameters Sets](#parameters-sets) section above). The default one will be used if none is specified
  - StartCheckingAt (optional): Only start checking this token after this date and time
  - RemoveAfter (optional): Stop checking and remove this token from the list automatically after the indicated date and time
  - Source (internal use, ignore): Used internally when a token has been added by the Telegram Sniper to keep track of the rule that added it
	
The tokens will be automatically removed from the list by the sniper when they're bought (or a buy failed).

## Plans for the next releases
- Wallet mirroring
- Explore arbitrage strategies (possibly using flash loans, etc)
- Graphical User Interface
- Add support for non EVM-compatible blockchains
- Open to suggestions
