`use` for every package installed via cargo, `use crate` for every custom package (using the file name, or file_name::public_struct) 

**evm.rs** allows you to interact with Multicall.sol via bit masking

**address_book.rs** is a file that whitelists multicall address, pool addresses and blacklisted pool addresses

**constants.rs** initiates U256 structures for 0 and 1 (both expressed as U256) 1 ETH, 1 Finney (=10^-3 ETH); Recall that 1 Finney > 1 Gwei

**gas.rs** build the tier list of gas prices from mempool. If it has problems to do so, it queries the gas prices to the node

**utilities.rs** has several util functions (well commented), also for generating and signing transactions (not for sending them)

**wallet.rs** crate for creating a local wallet (keypair of public and private) given a private key (omitting 0x)

**flashbots.rs** it has the Bundle structure and via submit() you're capable of sending or simulating a certain bundle. In both cases you perform a rpc call to the uri endpoint of the relayer and you hash the content of such rpc call (via sign_body()). This hash is set at the header "X-Flashbots-Signature" in the form of "your_flashbot_public_key:hash_signed_with_your_flashbot_pvt_key" and the body of this request is the rpc_call ({"jsonrpc":"2.0"}) 

**main.rs** tries to read the Config structure from lib.rs in order to set the environment variables, if it resolves, it launches `run()` of lib.rs

**compound.rs** has a structure called CethEthMarket and two implementations: the first one allows you to initiate the structure (pairs, address of the contract fetching rates, current exchange rate) and to fetch the current exchange rate. The other implementations has several getters that return the value of specific keys. This second implementations has also naive getamountout and getamountin methods; via `sell_tokens()` you are capable of minting/burning cETH in exchange of ETH

**weth_token.rs** disciplines minting/burning of wETH (it's the analogous of compound.rs), with the difference that the exchange rate between ETH and wETH is fixed 1:1 (that's why you don't have the method for retrieving the exchange rate). A cool thing is that it splits each crate (weth, ceth, uniswap etc) in order to set different implementations that return different protocol types. In this way you always know if you're working witha a uni pool or aa token.

**sushiswap.rs** interact with MasterChef (minter of sushi token) and checks the pending rewards (in sushi) of a specific LiquidityProvider address for a specific pool

**uniswap.rs** has the structures to initiate the uniswaprouter (in order to call also getAmountsOut) and the uniswappool. For uniswappool it has the functions to compute deterministically getAmountOut and getAmountIn. It has also the methods for asking to multiple pairs and multiple reserves making a call to the flashbots contract 0x5EF1009b9FCD4fec3094a5564047e190D72Bd511.
It has also a method for retrieving all the wETH based pools given a certain factory address (it performs batch calls via the flashbots contract).
Then it has another method that calls iteratively the previous one (fetching all the wETH based pools for a given factory) iterating over a set of factories. it has again the implementation "market" that is common for compound.rs and weth_token.rs

**markets.rs** implements the market struct. It has a method that allows to fetch the best pool where to swap by calling recursively the method getAmountIn (specifying a certain amountOut of the token you want in exchange): the amount in is the token you're selling. The same with best_ask_price() for buyign since it returns the pool associated with the highest amount out and creates a market graph