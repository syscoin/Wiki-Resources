![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/IssueNewTokenScreen.png)  

# Creating an SPT Token on Spark

#### CLICK TO EXPAND

<details><summary>1. Install Spark wallet</summary>

* Go to https://github.com/blockchainfoundryinc/syscoin-spark-wallet/releases  
download and install the latest Spark wallet.
* Let the Spark wallet synchronise with the Syscoin 4 Blockchain.
* Fund your Spark wallet with at least 1 Syscoin.  
Syscoin can be purchased at [Bittrex](https://international.bittrex.com/Market/Index?MarketName=BTC-SYS), [Binance](https://www.binance.com/en/trade/SYS_BTC), [Flyp.me](https://flyp.me/) or [Changelly](https://changelly.com/)
</details>

<details><summary>2. Open Create Token Menu</summary>

* Go to the Create Token Dialogue in the main menu.  
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/CreateTokenMenu.png)  
You will see the Issue New Token form with 9 fields:
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/IssueNewTokenDialogue.png)
</details>

<details><summary>3. Token Symbol</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/SymbolField.png)  
* The _Token Symbol_ field is a 3 to 8 character alphanumeric string used to identify your token on exchanges, wallets, block explorers and any application that supports SPT's. Symbols may contain letters or numbers only. All Symbols must be uppercase and not contain any special characters.    
* Some exchanges support a maximum of 5 characters.
* This is a REQUIRED field.    
>Examples:  
>```aA& ``` - is not a valid Symbol  
>```APX12``` - is a valid Symbol  
</details>

<details><summary>4. Public Value</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/PublicValue.png)  
* _Public Value_ is a text string that includes any information you would like quickly and easily accessible to users through block explorers, wallets or other applications that support SPT's that gives details about your token, such as a short description or reason for creation.
* Up to a maximum of 256 characters.  
* This is NOT a REQUIRED field.
</details>

<details><summary>5. Owner Address</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/OwnerAddress.png)  
* _Owner Address_ is the address used to fund the creation of your token, and the owner of the allocation of tokens created in the _Initial Supply of Asset_.  
* The dropdown will show only Syscoin addresses with a balance large enough to create a token.
* Choose one of the addresses in the dropdown.  
* This is a REQUIRED field.

</details>
<details><summary>6. Initial Supply</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/InitialSupply.png)  
* _Initial Supply_ is the number of tokens created in the initial allocation, when you first create the token.   
* These MUST NOT EXCEED the number of tokens in _Maximum Supply_.  
* These WILL NOT appear in the _My Tokens_ page, but in the _Distribute Tokens_ page.  
* These can only be distributed using the _Distribute Tokens_ function.  

</details>

<details><summary>7. Precision</summary>
  
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/Precision.png)   
* _Precision_ is the number of units that your token can be divisible by.  
> EXAMPLE:  
> Precision 0 = Units cannot be divided and will br represented by whole numbers only  
> Precision 8 = Units can be divided into units of 0.00000001 of the whole, exactly like Bitcoin.  
</details>
