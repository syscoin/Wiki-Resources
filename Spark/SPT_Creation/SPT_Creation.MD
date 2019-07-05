![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/IssueNewTokenScreen.png)  

# Creating an SPT Token on Spark

## INSTRUCTIONS (CLICK TO EXPAND)

<details><summary>Install Spark wallet</summary>

* Go to https://github.com/blockchainfoundryinc/syscoin-spark-wallet/releases  
download and install the latest Spark wallet.
* Let the Spark wallet synchronise with the Syscoin 4 Blockchain.
* Fund your Spark wallet with at least 1 Syscoin.  
Syscoin can be purchased at [Bittrex](https://international.bittrex.com/Market/Index?MarketName=BTC-SYS), [Binance](https://www.binance.com/en/trade/SYS_BTC), [Flyp.me](https://flyp.me/) or [Changelly](https://changelly.com/)
</details>

<details><summary>Complete the Create Token Form</summary>

* Go to the Create Token Dialogue in the main menu.  
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/CreateTokenMenu.png)  
You will see the Issue New Token form with 9 fields:
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/IssueNewTokenDialogue.png)  
* You will need to complete each field of this form with the specifications and parameters of your token. 
#### More information on each form field is detailed below:
___
</details>

### FORM FIELD DETAILS

<details><summary>1. Token Symbol</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/SymbolField.png)  

The _Token Symbol_ field is a 3 to 8 character alphanumeric string used to identify your token on exchanges, wallets, block explorers and any application that supports SPT's. Symbols may contain letters or numbers only. All Symbols must be uppercase and not contain any special characters.    

* This is a REQUIRED field.    
* Some exchanges support a maximum of 5 characters.  
>Examples:  
>```aA& ``` - is not a valid Symbol  
>```APX12``` - is a valid Symbol  
</details>

<details><summary>2. Public Value</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/PublicValue.png)  

 _Public Value_ is a text string that includes any information you would like quickly and easily accessible to users through block explorers, wallets or other applications that support SPT's that gives details about your token, such as a short description or reason for creation.  

* This is NOT a REQUIRED field.  
* Up to a maximum of 256 characters.  
</details>

<details><summary>3. Owner Address</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/OwnerAddress.png)  

_Owner Address_ is the address used to fund the creation of your token, and the owner of the allocation of tokens created in the _Initial Supply of Asset_.  

* This is a REQUIRED field.  
* The dropdown will show only Syscoin addresses with a balance large enough to create a token.
* Choose one of the addresses in the dropdown.  


</details>
<details><summary>4. Initial Supply</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/InitialSupply.png)  

_Initial Supply_ is the number of tokens created in the initial allocation, when you first create the token.   

* This is a REQUIRED field.  
* These MUST NOT EXCEED the number of tokens in _Maximum Supply_.  
* These WILL NOT appear in the _My Tokens_ page, but in the _Distribute Tokens_ page.  
* These can only be distributed using the _Distribute Tokens_ function.  

</details>

<details><summary>5. Precision</summary>
  
![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/Precision.png)   

 _Precision_ is the number of units that your token can be divisible by.  

* This is a REQUIRED field.  
* Choose a number between 0 and 8 from the dropdown.  
* Precision also affects _Maximum Supply_. There are a maximum 18 decimal places. Precision 0 will allow 9^18 indivisible tokens, Precision 1 will allow 9^17 tokens divisible by 10, Precision 2 will allow 9^16 tokens divisible by 100 etc.
* Default is set to 8 (Bitcoin Precision)

> EXAMPLE:  
> Precision 0 = Units cannot be divided and will be represented by whole numbers only.  
> Precision 8 = Units can be divided into units of 0.00000001 of the whole, exactly like Bitcoin.  
</details>

<details><summary>6. Maximum Supply</summary>

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/MaximumSupply.png)  

_Maximum Supply_ is a number you set as the maximum number of your tokens that can ever be created.  

* This is a REQUIRED field.  
* You can set your maximum supply to any number between 1 and 1^18 (depending on _Precision_).  
* Clicking the blue _maximum supply_ button will autofill the form field with the the highest number allowed to be created.  
* Your _Initial Supply_ will be taken out of this number.  
* _Total Supply_ can never exceed _Maximum Supply_.    
> Example:
> Maximum supply = 1000000, Initial supply=1000. Total supply=1000
</details>

<details><summary>7. Update Flags</summary>  

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/UpdateFlags.png)  
_Update Flags_ set editable token parameters. Each permission flag has a unique number, the sum of which define  parameters the owner can change after token creation. 
 
| FLAG | EDITABLE |   
| --- | --- |  
| 1 | Admin Status |  
| 2 | Can Edit Public Field |   
| 4 | Can Edit Smart Contract (Ethereum) Address | 
| 8 | Can Update Supply |   
| 16 | Can Change Flags (Admin Status Required) |  

* This is a REQUIRED field.
* All flags (Full permissions, or 31) is the default setting.
> Example:
> The creator wants to be able to update total supply (mint new tokens) and the description, but does not want the  Ethereum smart contract to ever be editable, so they choose flags 2 and 8 (Can edit public field and supply) only.   
> 10 _(2+8=10)_ = Can edit public field and update supply but no other permissions.  
>
> 31 _(1+2+4+8+16)_ = Full permissions.

</details>

<details><summary>8. Witness</summary>  

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/Witness.png)  
_Witness_ is a Syscoin address that will sign as a witness to the creation of the token for web-of-trust notarization of this transaction.
* This is NOT a REQUIRED field.  
</details>

<details><summary>9. Contract</summary>  

![](https://github.com/syscoin/Wiki-Resources/blob/master/Spark/SPT_Creation/images/Contract.png)  
_Contract_ is the address for the Ethereum token contract used on the Ethereum side of the Syscoin Bridge. 
* This is NOT a REQUIRED field.  
* Must be in hex and not include the '0x' format tag.  
* Leave empty for no smart contract bridge.  
> Example: contract '0xb060ddb93707d2bc2f8bcc39451a5a28852f8d1d' should be set as:  
>'b060ddb93707d2bc2f8bcc39451a5a28852f8d1d'. 

</details>
