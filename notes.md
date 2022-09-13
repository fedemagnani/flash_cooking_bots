`use` for every package installed via cargo, `use crate` for every custom package (using the file name, or file_name::public_struct) 
**evm.rs** allows you to interact with Multicall.sol via bit masking
**address_book.rs** is a file that whitelists multicall address, pool addresses and blacklisted pool addresses
**constants.rs** initiates U256 structures for 0 and 1 (both expressed as U256) 1 ETH, 1 Finney (=10^-3 ETH); Recall that 1 Finney > 1 Gwei
**gas.rs** build the tier list of gas prices from mempool. If it has problems to do so, it queries the gas prices to the node
**utilities.rs** has several util functions (well commented), also for generating and signing transactions (not for sending them)
**wallet.rs** crate for creating a local wallet (keypair of public and private) given a private key (omitting 0x)