![ET sniper](logo1.jpeg)

[Download the latest version here](https://github.com/devareus/Eclectic-Token-Sniper/releases)
# Eclectic-Token-Sniper
Eclectic Token Sniper (ETSniper) is a versatile token sniper for the blockchain. It should work with any decentralized exchange compatible with the Uniswap V2 API on any blockchain compatible with Ethereum. For example, just to mention a few:

#### Chains
- Ethereum
- EthereumPoW (ETHW)
- BNB Smart Chain (former Binance Smart Chain)
- opBNB
- Avalanche C-Chain
- Polygon
- Fantom Opera
- Harmony ONE
- Dogechain
- EthereumPoW
- Huobi ECO
- Proof of Memes - POM
- Exosama
- Flare
- Arbitrum One
- CoreDAO
- Base (Coinbase)
- Linea

#### DEXs
- UniswapV2
- SushiSwap
- PancakeSwap
- ApeSwap
- Biswap
- BakerySwap
- Traderjoexyz
- Dogeswap

The Honeypot Check functionality is available for the Ethereum, Binance Smart Chain, opBNB, Polygon, Fantom, Avalanche, Dogechain, EthereumPoW, Huobi ECO, Proof of Memes - POM, Exosama, CoreDAO, BeanEco SmartChain, PulseChain, Flare, Base (Coinbase), Arbitrum One and Linea (including their mainnets and some testnets).

The Safe-buy functionality is available for the Binance Smart Chain, Polygon, Fantom and Dogechain (mainnets and testnets).

I can add support for others on request. Send me (0xa9f5B4Fd93ddA4eb247CC1cC726a6602a2Ce7F00) enough native tokens of the corresponding blockchain so I can deploy the contracts and get in touch!

## Main features

- Support for multiple chains, decentralised exchanges and liquidity pairs.
- Support for EIP1559 gas fees in the blockchains that implement it.
- Automatic selection of the liquidity pair with the best price when buying.
- Able to monitor, buy and sell multiple tokens from different sources simultaneously.
- Can snipe directly from token contract addresses as well as from liquidity pair addresses
- Multiple methods for sniping tokens:
	- From a list of token addresses to watch.
	- From PairCreated events.
	- From AddLiquidity transactions from confirmed blocks or mempool.
	- From Telegram channels/groups or private messages, including private ones and obfuscated addresses.
	- Customisable event rules.
	- Customisable rules that let you trigger a buy depending on sender or receiver addresses of a transaction, its method signature, values in the input, etc.
	- Dynamic activation of rules based on counters or flags.
- Tokens auditing, including:
	- Very quick and efficient in-house and in-chain honeypot check, that checks if a token can be approved and sold, with configurable limits for buy and sell taxes and gas usage.
	- Contract source verification and filtering by a configurable list of expressions.
	- Minimum and maximum liquidity amount and minimum percentage.
	- Running external programs for additional checks.
- Optional buy using a safety contract that checks for honeypot, liquidity and drawdown within the same buy transaction and reverts it back if the token doesn't fulfil the requirements.
- Whitelisting of token addresses.
- Blacklisting of addresses, symbols and names.
- Stops buying tokens when the account balance is below a configurable threshold.
- Monitoring of bought tokens that supports:
	- Profit estimation, including estimation of sell tax and gas.
	- Taking profit after reaching a certain value.
	- Setting a fixed stop loss or based on the estimated drawdown.
	- Setting a trailing stop.
	- Disabling the sell of a token for a certain time after being bought.
	- Selling a token after a certain amount of time after buying.
	- Selling a token if the trailing stop doesn't change after a certain amount of time.
	- Mempool monitoring to frontrun RemoveLiquidity transactions.
- Simulation mode to test your sources and settings without risking any real funds.
- Support for Polygon-edge gRPC TxPool (Dogechain)

***

For bug reports, feedback or help, use the Issues functionality here on Github, send an email to [devareus@protonmail.com](devareus@protonmail.com) or join the
Telegram channel at [http://t.me/EclecticTokenSniper](http://t.me/EclecticTokenSniper).

A 1.5% developer fee is paid on each sucesfull buy. No developer fees are paid when selling.
 
Buying using the safe-buy contract has an extra fee of 1.5% of the amount paid if successful. Nothing is charged otherwise.

***If for privacy, costs or any other reasons you prefer not to pay per-transaction fees, get in touch and we can discuss providing you with a version free of fees, for a fixed price or subscription-based***

If you want to support further development of this sniper you can send a donation on Ethereum or BSC to: 

0xa9f5B4Fd93ddA4eb247CC1cC726a6602a2Ce7F00


## How to run it
Download the corresponding executable for your platform from the Releases section and place it in an empty folder. At the moment there are binaries available for Windows x64, Linux x64, Linux ARM (32 and 64 bits) and MacOS. If you need binaries for another platform, get in touch and I'll try to build one.

Download the appsettings-template.json file (or appsettings-telegram_template.json, a bit simplified and more straightforward version for just Telegram snipping), copy it as appsettings.json and put in the same folder as the executable.
Edit it following the instructions below.

Double click on the executable, or better, open a terminal/console and run it from it.

You can quit the sniper by pressing Ctrl+C. The status of the sniper is saved each time a token is bought, sold or the trailing stops are changed, so if you launch it again from the same folder, it will carry on monitoring them.

If you want to run the sniper on different chains, exchanges or LiquidityPairAddress, it's recommended to run each from a different folder.

## appsettings.json (v230818)

NOTE: The JSON standard doesn't allow comments, but the library I use does, so I used them to add clarity to the appsettings template. Feel free to remove them if they bother you.

### Accounts
  - WalletPrivateKey: Your account private key. It's a string of numbers and letters that you can export, from example, from your Metamask. *It's not the list of recovery words that gives access to all the accounts in your Metamask, but a separate key for each account in your wallet*. It's recommended to use a separate account so the sniper doesn't interfere with your normal activity or is affected by it. Don't hold in this account more funds than you need.
  - BlockExplorerApiKey: If you want to check if a token's contract is verified, you'll need to get an API key from the block explorer (bscscan.com for the BSC) and put it here.

### Node
  - NodeAddressHttp: The RPC endpoint of the blockchain node that you'll connect to. You can use one of the official nodes, get a node from moralis.io or other providers or deploy your own node.
  - NodeAddressWss: For some snipping options you'll also need a websocket node endpoint. You can use the nariox node (not recomended, as they don't seem to support events filters), get one from moralis.io or other provider. Or if you're serious about snipping, particulary from the mempool, deploy and use your private node and run the bot as close as possible to the node for miminum latency.
  - NodeAddressgRPC: The gRPC endpoint of the node. Required for Polygon-edge derived blockchains, like Dogechain.
  - UsegRPCforMempool: Use gRPC TxPool instead of standard WebSocket Mempool. Set to true for Polygon-edge derived blockchains, like Dogechain. Otherwise, leave as false.
  - GetContractSourceUrl: The URL for downloading the tokens' contracts' source codes from a blockchain explorer.
  - NativeTokenSymbol: Symbol of the native token of the chain (BNB, MATIC, FTM, AVAX, Metis, ONE, etc). You can generally leave it blank and the bot will find it out automatically. But with some non-standard DEXs you might need to set it manually.

### DEX
  - ExchangeRouterAddress: The contract address for the exchange's router. For example, for PancakeSwap it's "0x10ed43c718714eb63d5aa57b78b54704e256024e".
  - LiquidityTokens: By default, the native token of the blockchain (BNB, for example) will be used directly to buy the tokens (if there's enough liquidity in the pair). Alternatively, you can add a list of additional liquidity tokens that will be used to try to find liquidity for the snipped tokens. If liquidity is found, the *AmountToSnipe* of native tokens will be swapped for one of these intermediary tokens before buying the snipped token. For example, if liquidity is found with "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56" on the BSC (BUSD), the *AmountToSnipe* amount of BNB will be converted to BUSD and these used to buy the snipped token.
  - AlwaysIncludeBaseTokenInLiquidityTokens: Leave enabled (also the default if not present) to add the base token automatically to the list of available liquidity tokens.
  
### Global settings
  - TransactionRevertTimeSeconds: Maximum time for a transaction to be confirmed before it's automatically cancelled.
  - MonitorPricesEverySeconds: When 0, it disables the monitoring of the prices of the tokens the sniper has bought. If set to a number, the sniper will check the prices regularly at the set interval (in seconds).
  - WatchTokensInterval: Indicates the interval, in seconds, at which the tokens in the watched list will be checked for sniping. Each time the interval elapses a new token from the list will be checked, starting again for the beginning once the list is exhausted.

### Simulation mode

  - SimulationMode: If enabled, the bot will run in simulation mode, without submitting any real transactions. This can be useful to see if your sources or settings can make consistent profits, without having to risk your real money while you adjust them. Mind that some things, like how other transactions in the current block will impact prices, can't be simulated so the simulated results could differ significantly from what would have happened in reality. Simulations will be more accurate on blockchains where the HoneypotCheck contract is deployed and if you have a real balance in your wallet of at least *AmountToSnipe*.
  - SimulationModeWalletBalance: Initial simulated balance the the first time you run the bot in simulation mode or after deleting the *simulation-balance.json* file.
  - SimulationModeBuyDelay: Simulated time for your buy transactions to be processed. The token price used for your simulated transaction will depend on this. It's recommended to set it to the block time of the chain or around that.
  - SimulationModeApproveDelay: Same as the above, but for approve transactions.
  - SimulationModeSellDelay: Similarly, for sell transactions. Will determine what token price will be used to simulate your sells.

### Snipers
  - WatchedTokensSniper: Enables or disables the watch list. See [Watch List](#watch-list) section below.
  - TelegramSniper: If enabled, the sniper will listen on the configured Telegram channels, filtering by keywords and sniping token addresses for it. Use only with reliable sources.
  - PairCreatedEventSniper: If enabled, the sniper will listen for PairCreated events for new tokens to snipe.
  - AddLiquiditySniperBlocks: If enabled, the sniper will listen for all AddLiquidity transactions on the DEX for tokens to snipe. This method is not recommended unless you limit buys to the white list, or you'll end buying lots of old tokens.
  - AddLiquiditySniperMempool: If enabled, the sniper will monitor the mempool for AddLiquidity transactions to try to buy in the same block or the following adjusting the buy gas price accordingly.
  - TransactionSniperBlocks: Enables a sniper based on flexible rules ([see below](#customisable-transaction-sniper)) analising confirmed transactions.
  - TransactionSniperMempool: Same as above, but for transactions in the mempool. See [Customisable Transation Sniper](#customisable-transaction-sniper).
  - EventSniper: Enables a sniper based on flexible rules analising events. See [Customisable Event Sniper](#customisable-event-sniper).
  - PairCreatedEventSniperParametersSet: ParametersSet to use for the PairCreated events sniper.
  - AddLiquiditySniperBlocksParametersSet: ParametersSet to use for the AddLiquiditySniperBlocks sniper.
  - AddLiquiditySniperMempoolParametersSet: ParametersSet to use for the AddLiquiditySniperMempool sniper.
  - AddLiquiditySniperMempoolMaxGasPrice: Maximum gas price when buying from the AddLiquiditySniperMempool sniper.
  - AddLiquiditySniperMempoolGasPriceAdjustment: By default the bot uses the same gas price as the transaction found in the mempool. It's not recommended to increase it because as you risk front running the transaction and the buy will fail because the liquidity addition isn't effective yet. And if you set a negative number (lower the gas price) other bots will most surely buy before you.

### Buy settings
- BuyEnabled: Lets you disable the buy of tokens globally. This can be toggled while running by pressing the B key, but it will revert back to the default configured here if the sniper is restarted.
- MinWalletBalance: Minimum balance in your account for buying tokens. The sniper won't buy any more tokens if your balance is below this figure. If the balance increases (for example, you send more funds, or a token is sold) it will start buying again. I recommend to set this to slightly more than the *AmountToSnipe*, to guarantee that some funds are left to pay for the networks fees when selling.
- OnlyBuyWhitelisted: Will only buy tokens included in the white list.

### Sell settings
- SellEnabled: Lets you disable the sell the tokens globally. This can be toggled while running by pressing the S key, but it will revert back to the default configured here if the sniper is restarted.
- MempoolRemoveLiquidityEmergencySell: If enabled, the mempool is monitored for RemoveLiquidity transactions of the owned tokens. If one is detected the bot will try to frontrun the removal transaction to sell the tokens before the liquidity is gone. Mind that any RemoveLiquidity of the token will trigger this, even if it's only partial, so false positives are possible.
- EmergencySellGasPriceAdjustment: Adjustment of gas price for emergency approvals and sells, in relation to the gas price of the RemoveLiquidity transaction that triggered them. Should be at least 1 to have any chances of frontrunning it. The higher the value the better chances of selling before the liquidity is gone, but mind the costs!
- EmergencySellMaxGasPrice: Maximum gas price for emergency approvals and sells. This is a safe net in case a RemoveLiquidity had a crazy gas price.
- EmergencySellSlippage: Maximum slippage for emergency sells. You probably want a bigger percentage than usual here, as it's better to recover some of your investment than nothing.

### White/Black Listing
- WhitelistedTokens: This list of token addresses can have two different but similar usages. If *OnlyBuyWhitelisted* is enabled, only the tokens in this list can be bought. If it's disabled, it skips any audits for these tokens (use with care!).
- BlacklistedTokens: A list of token addresses to never buy.
- BlacklistedTokenSymbols: Tokens whose symbols contain these strings won't be bought. This can be use to filter out tokens with symbols that contain, for example, "doge", "elon", "musk", "shiba", etc, as these are usually scams, or "test" which usually are... well... that... tests :)
- BlacklistedTokenNames: Same as above, but for the token names.
- ContractRedFlagStrings: If contract verification is enabled, this is the list of expressions that prevent buying a token if any of them is present in its source code.

### Telegram Sniper
- TelegramApiId: Telegram Account Id for the Telegram sniping method.
- TelegramApiHash: Telegram Account Hash for the Telegram sniping method.
- TelegramApiPhoneNo: Telegram Phone Number for the Telegram sniping method.

*To obtain your Telegram API credentials follow the instructions here: [https://core.telegram.org/api/obtaining_api_id](https://core.telegram.org/api/obtaining_api_id). **Use at your own risk as whether Telegram allows this kind of use is a bit of a grey area and they could ban you forever from their service***

-  TelegramSilenceChatsList: Set to true if you don't want to see the list of Telegram channels, groups or converstations you're connected to, each time that you start the bot.
-  TelegramListenerConfiguration: This a list of rules for sniping tokens from Telegram. For each rule you can set:
   - RuleName: A name for this rule, for your reference.
   - Action: Can be *buy*, *sell* or *none* (the default value for Telegram rules is *buy*). To sell a token it must be present in the tokens-current.json list at moment of selling (usually bought previously by another rule or snipping method). *none* can be used if you only want to use the rule for [Counters](#Counters).
   - ChatId: The *username* (the part after *t.me/* URLs, for public ones) or the numerical ID (for private ones) of the chat to listen to for this rule. You must already be a member of the channel/group or have a private conversation opened. You can obtain these from the list shown when you start the Telegram sniper.
   - AdminsOnly: Enable it to only get addresses from messages of the admins of the chat.
   - IgnoreBots: Usually necessary if using the previous option, as bots are normally admins and you don't want to buy a token when a bot mentions a user that has a token address in his name.
   - SolveMathExpressions: Enable to solve math expressions in the messages when searching for contract addresses. For example, 0xae13d(4+5)89daC2f0dEbFf460aC112a837C89BAa7cd would be solved into 0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd.
   - IgnoreBlankSpace: Enable to ignore all blank space when searching for contract addresses. This helps with addresses split across several lines.
   - UseAddressNumber: Indicates which address within the message to snip. 1 for the first one, 2 for the second, etc. If not present or set to 0, all the addresses present in a rule-matching message will be sniped. Negative numbers to start counting from the bottom of the message (ie, -1 for last address, -2 for the 2nd last, etc)
   - RequiredText: All these words or expressions need to be present in a message for it to be parsed in search of a token address.
   - ExcludeText: If any of these words or expressions is present in a message, it will be ignored and not used for snipping.
   - EnableRegexPatterns: Set to true (default is false) to use regular expressions in RequiredText and ExcludeText.
   - ParametersSet: The set of parameters for this rule (see the [Parameters Sets](#parameters-sets) section above). The default one will be used if none is specified.
   - AddToWatchList (optional): If *true* the token address will be added to the watch list if the buy fails when the announcement is received. This is useful if a token is announced before is available for trading.
   - RemoveFromWatchListAfterMinutes (optional): If *AddToWatchList* is enabled, this setting allows you to set a number of minutes after which the token will be removed from the watch list if it not bought yet.

### Customisable Transaction Sniper
This feature allows you to buy or sell tokens when a transaction is found (either confirmed in the blockchain or in the mempool) that matches certain conditions. This can be used to mirror wallets, snipe a token when the token's developer call a certain method in a contract and probably many other uses. Once *EclecticSniperBlocks*, *EclecticSniperMempool* or both have been enabled, you can set a list of rules in *EclecticSniperConfiguration*. All the conditions in a rule have to be true for the rule to be triggered; usually you'll only set a few. The allowed settings are:

- RuleName: A name for this rule, for your reference (required).
- Action: Can be *buy*, *sell* or *none* (the default value, if not present, is *none*). To sell a token it must be present in the tokens-current.json list at moment of selling (usually bought previously by another rule or snipping method). *none* can be used if you only want to use the rule for [Counters](#Counters).

The bot can obtain the address of the token to buy or sell using one of the following options:

- Token: Predetermined fixed address.
- TokenFromInputOffset: Instead of setting a fixed token to buy with the previous option, you can obtain the address from the input data of the triggering transaction by specifying its offset here (the position of the first byte of the address, starting counting from 0)...
- TokenFromToAddress: Use the "To" address of the triggering transaction.
- TokenFromTransferEvent: Try to find the token address in the Transfer events emitted by the transaction (make sure you've blacklisted the most usual liquidity tokens (BUSD, USDT, USDC, etc)).
- TokenFromApprovalEvent: Try to find the token address in the Approval events emitted by the transaction (make sure you've blacklisted the most usual liquidity tokens (BUSD, USDT, USDC, etc)).

Any number of the following conditions can be set for each rule:

- Origin: Can be *block*, *mempool*, *logs* (see [Customisable Event Sniper](#customisable-event-sniper)) or *any*.
- From: If present, the sender of the transaction must be the specified address.
- To: If present, the destination of the transaction must be the specified address (usually a DEX or Token contract).
- FromIn: If present, the sender of the transaction must be one of the addresses in the provided list.
- ToIn: If present, the sender of the transaction must be one of the addresses in the provided list.
- MethodByteSignatureIs: If present, the method's byte signature (first 8 bytes of the Input data) must match this.
- MethodTextSignatureIs: If present, the method's signature (in text form) must match this.
- Input: Sets a number of conditions that certain data in the Input of the transaction must match. The available options are:
  - Offset: Position of the data (counting from 0) (required)
  - Length: Length of the data (for example, 40 for addresses) (required)
  - Is, IsNot, IsLessThan, IsLessThanOrEqual, IsGreaterThan, IsGreaterThanOrEqual: The condition/s that the value must fulfil. The values are specified in hexadecimal, whether the prefix "0x" is used or not.
  - IsIn: The value must be one of the list.
  - IsNotIn: The value can't be any of the list.

Additionally, the following settings are available:

- GasPriceFromTxAdjustment: If present, the gas price of our trade transaction will be calculated from the gas price of the triggering transaction, plus or less the value indicated here. Usefull usally when snipping from the mempool: a positive number will try to frontrun the triggering transaction. A negative value will try to get the transaction confirmed after the triggering one. A value of 0 will try to get the trade transaction confirmed in the same block, but there's always a risk of frontrunning the triggering transaction. If not present, the fix *BuyGasPrice* or *SellGasPrice* value in the Parameters Set will be used.
- ParametersSet, AddToWatchList and RemoveFromWatchListAfterMinutes: They work exactly as for the [Telegram Sniper](#telegram-sniper).

As a very simple example, the following rule would buy any token that receives a `enableTrading()` transaction, trying to buy in the same block:

        {
          "RuleName": "BuyOnEnableTrading",
          "Action": "buy",
          "Origin": "mempool",
          "MethodTextSignatureIs": "enableTrading()",
          "TokenFromToAddress": true,
          "ParametersSet": "mempool",
          "GasPriceFromTxAdjustment": 0
        }


The following is a bit more advanced example, that buys the token "0xABCDEF...0123456789" when its transfer and hold limits are set above certain amounts:

      {
        "RuleName": "BuyOnSetLimits",
        "Action": "buy",
        "Origin": "block",
        "ToIs": "0xABCDEF...0123456789",
        "MethodTextSignatureIs": "Set_Limits_For_Wallets(uint256,uint256)",
        "Input":[
          {
            "Offset": 8,
            "Length": 64,
            "IsGreaterThan": "0x10000000"
          },
          {
            "Offset": 72,
            "Length": 64,
            "IsGreaterThan": "0x50000000"
          }
        ],
        "TokenFromToAddress": true
      }

Quite similar to the previous one, this example tries to sell the token ***before*** its transfer limit is reduced:

      {
        "RuleName": "SellOnSetLimits",
        "Action": "sell",
        "Origin": "mempool",
        "ToIs": "0xABCDEF...0123456789",
        "MethodTextSignatureIs": "Set_Limits_For_Wallets(uint256,uint256)",
        "Input":[
          {
            "Offset": 8,
            "Length": 64,
            "IsLessThan": "0x10000000"
          }
        ],
        "TokenFromToAddress": true,
        "ParametersSet": "mempool",
        "GasPriceFromTxAdjustment": 50
      }

***Note: Mind that the *Set_Limits_For_Wallets(uint256,uint256)* is not standard and tokens might have another similar method or no equivalent at all.***

### Customisable Event Sniper

The bot can listen for Events in the blockchain for tokens to sell or buy, using some simple rules.

- RuleName: A name for this rule, for your reference (required).
- Action: Must be *buy*, *sell* or *none* (the default value, if not present, is *none*). To sell a token it must be present in the tokens-current.json list at moment of selling (usually bought previously by another rule or snipping method). *none* can be used if you only want to use the rule for [Counters](#Counters).
- AddressIs: The address of the contract emitting the event (optional).
- Topic0Is, Topic1Is and Topic2Is: Topics filters. In most cases you'll set two: topic 0 with the event type and either topic 1 or 2 as the origin or destination addresses, but this will vary depending on the interface defined by the developer of the contract that emits the event.
- TransactionFromIs: If present, the sender of the transaction to which the event belongs must be the specified address.
- TransactionToIs: If present, the destination of the transaction to which the event belongs must be the specified address (usually a DEX or Token contract).
- TransactionFromIsIn: If present, the sender of the transaction to which the event belongs must be one of the addresses in the provided list.
- TransactionToIsIn: If present, the sender of the transaction to which the event belongs must be one of the addresses in the provided list.
- MinTxValue: If present, ignores transactions below this value.

  The bot can obtain the address of the token to buy or sell using one of the following options:

- Token: Predetermined fixed address.
- TokenFromAddress: Use the address of the triggering event.
- TokenFromTopicNo: 0, 1 or 2. Gets the token address from one of the event's topics.
- ProcessTransaction: Use the customisable transaction rules for further analysis of the transaction that triggered the event. The combination of the Event Sniper + Transaction Sniper rules (but with AddLiquiditySniperBlocks off) allows the bot to only receive and analyse relevant transactions, instead of analysing every single mined transaction in every block, saving in both computing resources and node requests. But in many cases, you won't even need this, as you'll have enough information in the event and could use the previous options.

- BuyPercentageOfTxValue: If set, the bot will calculate the amount to buy based on the value of the transaction that triggered the event, up to the maximum amount specified in the AmountToBuy setting. For instance, if the transaction's value was 1 BNB and this is set to 50:
  - AmountToBuy is 0.75 -> 0.5 BNB will be bought
  - AmountToBuy is 0.25 -> 0.25 BNB will be bought
- MirrorPercentageSoldByAddress: If set, the bot will sell the same percentage of the balance of tokens of the mirrored wallet that were sold.
       
And, as usual, you can also set:

- ParametersSet, AddToWatchList and RemoveFromWatchListAfterMinutes: They work exactly as for the [Telegram Sniper](#telegram-sniper).


### Counters

You can declare counters that allow for dynamic activation of rules. Apart from for actual counting, these counters can be used as flags (set to 0 or 1, for example). Playing with several rules, counters and conditions quite simple (and elaborate!) strategies can be achieved. For example, you could buy a token only after a number of buys and sales have been done, sell a token after a number of sales have been observed, buy a token after a method has been called but only after we've seen the token announced in a telegram rule, buy or sell after seeing certain transactions or events in succession...

There are two types of counters: global (which store one single common value) or per token (a separate value is stored for each token address). To declare counters, add a section like this:

    "Counters": [
      {
        "Name": "counter1",
        "Type": "global"
      },
      {
        "Name": "counter2",
        "Type": "perToken"
      }
    ],

Now you can use those counters/flags on any Telegram, transaction or event rule. To adjust/set a counter add to the rule a section like the following:

        "AdjustCounters": [
          {
            "Name": "counter1",
            "Inc": 2
          },
          {
            "Name": "counter2",
            "Set": 1
          }
        ]

The valid options for each counter are: Set, Inc or Dec.

And to enable a rule only when some counter condition/s are satisfied:

        "CountersConditions": [
          {
            "Name": "counter2",
            "IsGreaterThanOrEqual": 10
          }
        ]

The valid options for each counter are: Is, IsNot, IsLessThan, IsLessThanOrEqual, IsGreaterThan and IsGreaterThanOrEqual.

### Additional Settings

- LogParametersSets: If enabled computes and dumps the values of all the ParametersSets.
- RunNodeLatencyTest: If enabled, the average latency with the node will be calculated and shown when the bot is started.
- LogProcessedBlocks: If enabled, the bot will show the block number, timestamp and delay on reception of each received block (only if any type of block snipping is enabled).
- DeadWallets: List of dead wallets used for the calculation of the percentage of the total supply as liquidity. It's recommended not to touch this.

You can ignore the rest of the file, it's mainly log files configuration.

## Parameters Sets

This section allows you to define sets of parameters to be used for the different snipers, watched tokens or Telegram rules. At the very least there needs to be a *"default"* set (it needs to be literally named *"default"*), which will be used by the snipers unless another set is specified. All the parameters need to be set for the default one. The other sets can consist of just a few settings if so desired, in which case the *"default"* values will be used for those missing.

The available settings are:

### Gas settings
  - BuyGasPrice: Gas price of buy transactions.
  - ApproveGasPrice: Gas price for approval transactions.
  - SellGasPrice: Gas price for sell transactions.
  - BuyGasAmount: Maximum gas amount for buy transactions.
  - ApproveGasAmount: Maximum gas amount for approval transactions.
  - SellGasAmount: Maximum gas amount for sell transactions.
  - BuyEip1559MaxPriorityFee: Sets the EIP1559 priority fee for buy transactions. Set to -1 to disable EIP1559.
  - ApproveEip1559MaxPriorityFee: Sets the EIP1559 priority fee for approval transactions. Set to -1 to disable EIP1559.
  - SellEip1559MaxPriorityFee: Sets the EIP1559 priority fee for sell transactions. Set to -1 to disable EIP1559.
  - EstimateGasOnBuy: If enabled, an estimation of gas is done before submitting buy transactions and checked against the maximum amount above. This has the advantage of not wasting gas in aborted transactions if these are going to fail due to being out-of-gas, but the disadvantage of making the snip operation slower. You can leave it off if using the HoneypotCheck functionality, as the gas limits are already checked by it.
  - EstimateGasOnSell: Similar to the above, but for sell transactions.
   
### Buy settings
- BuyEnabled: Enables or disables the buy of tokens for the sniper, rule or watched token using this set.
- AmountToSnipe: The amount to buy of each snipped token (in the native token of the chain, BNB for example in the case of the BSC).
- FindMaxAmountCanBuy: If enabled, the bot will try to estimate the maximum amount (up to AmountToSnipe) that it can buy of the token. This option is helpful for when the tokens' devs set a maximum amount of tokens that can be bought.
- FindMaxAmountCanBuySteps: Number of steps used for the max amount estimation. The higher the number, the more precise the estimation will be, but at the cost of requiring more requests with the node and taking longer. The default if this option is omitted is 5.
- BuySlippage: Maximum slippage for buying.
- AlwaysFindBestLiquidityPair: If enabled, the bot will always try to find the liquidity pair with the best price for the amount to buy, from the list provided above. If disabled, the bot will use the liquidity pair in the AddLiquidity transaction or PairCreated event or the base token; for other snipping methods, the best liquidity pair will still be selected (in future releases a predefined paired token or an address extracted from the triggering event or transaction will be configurable too).
- AuditTokens: Enables the audit of tokens before buying. You can enable or disable the followings checks separately: 
  - HoneypotCheckEnabled: Using an in-house and in-chain solution checks if a token can be bought, approved and sold before buying. It can also check for maximum taxes, gas and drawdowns, using the limits set below. This option requires HoneypotCheck support for the blockchain. At the moment Ethereum, EthereumPoW, BSC, Polygon, Fantom, Avalanche and Dogechain are supported. If you need others, send us a request.
  - SimpleBuyCheck: Checks if it's possible to buy the token. This check is redundant if using the HoneypotCheck, but it might provide more details about the reason why buying is not possible.
  - CheckContract: Only buys if the token's contract is verified on BSCScan and it doesn't contain any of the expressions in the red flags list.
  - CheckLiquidity: Doesn't buy if the available liquidity doesn't fulfil the limits below.
 	- ExternalProgramCheck: Runs an external program for additional checks ([see below](#running-external-programs-for-additional-checks)).

**Mind that the tokens owners can change the taxes at any time! That a token passes an audit is not guarantee that you'll be able to sell the token later or you won't be charged higher taxes. And obviously, it doesn't guarantee that the owner won't do a rug pull and remove all the liquidity from the DEX at any some point.**

- MaxBuyTax: Used for the token audits. Maximum Buy Tax for sniping a token.
- MaxSellTax: Similarly, maximum Sell Tax for sniping a token.
- MaxTotalTax: Maximum total of taxes (buy+sell) for snipping a token.
- MaxDrawdown: The estimated *drawdown* is the estimated loss caused by the price impacts, taxes and gas fees if the token was bought and sold immediately. The bot won't buy a token if the estimated drawdown is higher than this.
- MinLiquidityInBaseTokens: Minimum liquidity in the pair to allow buying a token (if *CheckLiquidity* is enabled), expressed as the equivalent worth of base tokens.
- MaxLiquidityInBaseTokens: Maximum liquidity in the pair to allow buying a token (if *CheckLiquidity* is enabled), expressed as the equivalent worth of base tokens. Set to -1 for unlimited.
- MinLiquidityPercentage: Minimum liquidity, as a percentage of the total supply of the token, in the pair to allow buying a token (if *CheckLiquidity* is enabled).
- MinNewAddedLiquidityPercentage: Minimum liquidity, as a percentage of the total supply of the token, added by the AddLiquidity transaction (if *CheckLiquidity* is enabled). This applies only to *PairCreatedEventSniper* and *AddLiquiditySniperBlocks*.
- BuyDelaySeconds: Waits the indicated amount of seconds before buying a token. This is usually used to avoid antibots measures, but the Honeypot Check method is more reliable.
- BuyAndSellTest: Makes a small buy and sell just before buying the whole amount of the token, after all the audits (if enabled) have been passed, aborting the operation if the sell doesn't go through. This can be useful as a double check against honeypots (some scammers have learnt to avoid standard honeypot checks) or you want to use the bot in a blockchain where the honeypot check contract is not available (but contact me if you'd like this blockchain to be supported). Unlike audits, this test is done only once per token so as to prevent further spending; if it fails for any reason, the buy is cancelled, the token will be added to the "seen list" and, depending on your settings, ignored from this moment.
- BuyAndSellTestAmount: The amount to use for the test (in the native token of the chain, BNB for example in the case of the BSC).
- CheckSeen: If enabled, it will check again a token that has been previously bought or discarded, if the sniper finds it again. It's recommended to leave it disabled.
- BuySeen: If enabled, it allows buying a token again, even if it's been previously bought or discarded, if the sniper finds it again. It's recommended to leave it disabled.
- BuyOwned: If enabled, it allows buying a token again, even if it's currently owned, if the sniper finds it again. It's recommended to leave it disabled.
- UseSafeBuyContract: Buy using the safe-buy contract. If enabled, a special contract will be used to buy that will check for honeypot, liquidity and tax&price impact limits in the same transaction, reverting it if it's not safe to buy the token.

**Using this contract has an additional dev fee of 1.5% of the amount invested plus a higher cost in gas, as the contract will have to check liquidity and simulate a buy, an approval and a sell, before the effective buy, and all these tests will be done by the miner processing the transaction and not just a plain node like in the normal honeypot check (which doesn't consume gas), and the gas usage can be relatively high when buying a token with a complex transfer function.**

**For failed buys, depending on which step the checks fail you'll pay more or less gas: once a condition isn't met, the contract doesn't test the others so as to save some gas.**

**Unless you're snipping from the mempool or you really want to be as fast as possible in submitting a buy while still doing some checks you'll be better off by just using the plain honeypot check, which is slower but free.**

The following settings let you fine tune the safe-buy feature:

- SafeBuyCheckHoneypot: Simulates to buy, approve and sell before actually buying.
- SafeBuyCheckHoneypot: Doesn't buy if the liquidity is lower than *MinLiquidityAmount* above.
- SafeBuyCheckMaxLiquidity: Doesn't buy if the liquidity is higher than *MaxLiquidityAmount* above.
- SafeBuyCheckMaxTaxAndPriceImpact: Doesn't buy if the estimated loss due to taxes and price impact is higher than the parameter below.
- SafeBuyMaxTaxAndPriceImpact: Maximum estimated loss due to taxes and price impact. It's similar to the *MaxDrawdown* setting above, but doesn't consider the gas costs, as you're going to pay most of them anyway by running the buy&sell simulation within the buy transaction and calculating them would only increase the gas costs of using the contract for buying with very little gas saving if reverting the transaction because of it.

#### Running external programs for additional checks

Optionally, you can run an external program for additional audit options. For example, you could use curl or write a small script or use any other script or program provided by any 3rd party.

The available settings are the following:

- ExternalProgramCheck: Set to true to enable this feature.
- ExternalProgramPath: The name or path of the program to run.
- ExternalProgramArguments: The arguments to pass to the program, where {0} is replaced by the token address.
- ExternalProgramDefaultResult: The default result. This will usually be *false* for Success conditions and *true* for Fail conditions.

There are 4 available conditions that are evaluated in this exact order, although you'll usually only use one of them:

- ExternalProgramFailExitCode: Fails the audit if the exit code of the program matches this integer value.
- ExternalProgramFailOutput: Fails the audit if the console output of the program contains this string of characters.
- ExternalProgramSuccessExitCode: Fails the audit if the console output of the program contains this string of characters.

And you can enable or disable the following logs, which can be very useful while debugging your settings:

- ExternalProgramLogCommand
- ExternalProgramLogExitCode
- ExternalProgramLogOutput

A simple example of what you could do:

        "ExternalProgramCheck": true,
        "ExternalProgramPath": "curl",
        "ExternalProgramArguments": "https://some3rpartysite.com/api?chain=bsc&address={0}",
        "ExternalProgramFailExitCode": 0,

A more complex one:

        "ExternalProgramCheck": true,
        "ExternalProgramPath": "bash",
        "ExternalProgramArguments": "-c \"curl 'https://some3rpartysite.com/api/audit?chain=bsc&address={0}' | grep '\\\"blacklist\\\":false' | grep '\\\"taxModifiable\\\":false'",
        "ExternalProgramDefaultResult": false,
        "ExternalProgramSuccessExitCode": 0,

(although I'd recommend writing a separate shell script to avoid the pain of having to escape so many quotes).

### Sell settings

- SellEnabled: Enables or disables selling the tokens.
- TakeProfit: Sells a token if the current profit is above this figure. It's indicated as a percentage above the buy price, for example, 50 means "sell when the profit is above 50%", that is, the current price is about 150% of the buy price.
- StopLoss: Sets a stop loss. A token will be sold if its price goes below this percentage in relation to the buy price. For example, -25 means "sell when the price has gone down by a 25%".
- StopLossBasedOnDrawdown: Calculate the initial stop loss from the estimated drawdown (see an explanation of drawdown above). This option requires HoneypotCheck support for the blockchain and exchange. At the moment BSC and Polygon are supported. If you need others, send us a request.
- StopLossBasedOnDrawdownMargin: How much below the estimated drawdown to set initial stop loss if the previous option is enabled. For example, if the estimated drawdown of a token just bought is 17.75% and the margin is 12, the initial stop loss would be set to -27.75%.
- TrailingStopStrategy: If enabled and the price of a token increases above a threshold, the stop loss will be adjusted at a certain distance from the current price. It can be set to:

  - none: To disable it.
  - buy: the threshold and distance will be calculated as a percentage of the buy price.
  - current: the thresholds and distance will be calculated as a percentage of the current price.
  - profit: the thresholds and distance will be calculated as a percentage of the current profit.
  
- TrailingStopActivationThreshold: How much the price has to increase above the buy price for the stop loss to be changed.
- TrailingStopThreshold: How much the price has to increase above the previous stop loss for this to be changed.
- TrailingStopDistance: Distance at which the new stop loss will be set.
- SellSlippage: Maximum slippage set during selling.
- DontSellIfProfitBelow: If set, the bot won't sell a token if the current profit is below the configured percentage. This can be useful on blockchains with high gas costs, where you could end spending more on gas than what you'd get from selling a token with very little value left (for example, with -70 the bot won't sell if the token has lost 70% or more of its value since it was bought).
- DontSellBeforeMinutes: Prevents selling a token, even if the stop loss is touched, for the first few minutes after buying a token. The trailing stop (if enabled) won't be active during this time either. This can be used to prevent being taken out of the market by the initial volatility after a token is launched. Use at your own risk!
- SellAfterMinutes: Sells tokens after the amount of minutes indicated in this setting, no matter if any of the other conditions above is fulfilled or not. Set to -1 to disable.
- SellIfTrailingStopNotChangedInMinutes: Sells tokens if the trailing stop hasn't changed after the amount of minutes indicated in this setting, no matter if any of the other conditions above is fulfilled or not. Set to -1 to disable.
- ApplyGasTaxToProfitEstimations: If disabled (enabled by default) it won't deduct any gas fees or taxes from profit estimations
- ApplySellGasTaxToProfitEstimations: If enabled, an estimation of sell tax and approval and sell gas is deducted from the profit estimation used for the take profit, stop loss and trailing stop. This option requires HoneypotCheck support for the blockchain and exchange. At the moment BSC and Polygon are supported. If you need others, send us a request.
- RecheckSellGasTaxMinutes: Sets the frequency for updating the tax and gas estimations used for the above.
- ApprovalMethod: Defines when the approval to sell the tokens will be done. It can be set to one of the following:

	- preapprove: Sends the approve request just after the token is bought, except if the account had the token already approved.
	- check: At sell time, checks if the token is already approved and sends the transaction if it isn't.
	- always: At sell time, it just sends the approval request straight away so as to not waste any time in doing any checks.

For (mempool) emergency sells the *always* method will be used, unless the token has been pre-approved.

- WaitForApproval: If enabled, waits for the confirmation of the approve transaction before submitting the sell transaction. It's recommended to leave it disabled, unless for some estrange reason you're having frequent issues with approvals. For emergency sells approvals are never waited for, even if this option is set, as the point is to try to frontrun the liquidity removal.
- PercentageToSell: If set, sell only a percentage of the current balance of tokens. NOTE: This feature was a feature quickly added after a user requested it. It works, but the profit/loss reported will be incorrect until I add this factor to the calculation.

**Each bought token remembers the name of the set used when it was bought, even after restarting the bot; be careful if you change the sell settings of a parameters set for which there are still bought tokens being monitored as this will affect them. If you don't want to affect the sell parameters of already owned tokens, create and use a new set for new buys.**

## Watch List
Can be enabled with *WatchedTokensSniper*. The sniper will audit once and again the tokens in the *watched-tokens.json* file and will try to buy them once they pass the audit. This, with *AuditTokens* and *HoneypotCheckEnabled* enabled, is probably the most reliable method to snip known token addresses.

The list of watched tokens is stored in a file named *watched-tokens.json* (or *simulation-watched-tokens.json* for simulations). You can find a template in the release files. Multiple entries are supported, with each one having the following attributes:

  - Address: The contract address of the token to watch and snipe.
  - ParametersSet (optional): The set of parameters for this rule (see the [Parameters Sets](#parameters-sets) section above). The default one will be used if none is specified.
  - StartCheckingAt (optional): Only start checking this token after this date and time.
  - RemoveAfter (optional): Stop checking and remove this token from the list automatically after the indicated date and time.
  - Source (internal use, ignore): Used internally when a token has been added by the Telegram Sniper to keep track of the rule that added it.
	
The tokens will be automatically removed from the list by the sniper when they're bought (or a buy failed).

## Manual sell
You can instruct the bot to sell all the tokens or one token in particular by pressing the key *Z* in your keyboard (be patient, the menu might take a few seconds to show up, as the keys are only checked once per price monitoring cycle (which happens every *MonitorPricesEverySeconds*). The bot will show a list of currenly hold tokens. Enter a number to sell one token in particular, A for selling all the tokens or N to cancel and resume price monitoring. For manual sells, the only condition that is checked is *DontSellIfProfitBelow* if it's been set.
