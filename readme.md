# Project 5: Decentralized Star Notary  - DApp on Ethereum Rinkeby Network

Final project of Term 1 in Blockchain course. The project is built from the starter code provided for a Star Notary service utilising the ERC-721 standard. 



## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.


Pre-Requisite (Activities done before getting started )

1 ) Node installed 
2 ) Truffle , Metamask Installed
3 ) Infura Account creation
4 ) OpenZepplin-solidity & truffle-hdwallet-provider NPM installed & saved at Project directory.


## Smart Contract Functions

### 1 The smart contract tokens should have a name and a symbol. 


Token Name: [**Star Token Udacity**](https://rinkeby.etherscan.io/token/0x1545772b309944d09b6e71c49bda7978447340ff)  
Token Symbol: **STU**  


### 2 Implement the function: lookUptokenIdToStarInfo.

```

 return tokenIdToStarInfo[_tokenId].name;

```

The above function gets the star id and gets the name 'mapped' against that id.


### 3 Implement the function: exchangeStars

```
  address owner1 = ownerOf(_tokenId1);
  address owner2 = ownerOf(_tokenId2);
  require(owner1 == msg.sender || owner2 == msg.sender);
  _transferFrom(owner1, owner2, _tokenId1);
  _transferFrom(owner2, owner1, _tokenId2);

```

This function gets the owner of 2 accounts and validates the account belongs to that owner. Calls the transferFrom function to intiate the exchange.


### 4 Implement the function transferStar


```
    require(ownerOf(_tokenId) == msg.sender, "You can't transfer a Star you don't own.");
    _transferFrom(msg.sender, _to1, _tokenId); 

```

This function validates the owner and initates the transferFrom function.


## Add supporting Unit Tests


### The token name and token symbol are added properly.

```
    let tokenId = 6;
    await instance.createStar('New Star', tokenId, {from: accounts[0]});

    assert.equal(await instance.name.call(), "Star Token Udacity");
    assert.equal(await instance.symbol.call(), "STU");
```

Get the deployed instance and 'assert' if the name of the instance & symbol matches.

### 2 users can exchange their stars.


```
  let tokenId = 9;
    await instance.createStar('A Star', tokenId, {from: accounts[0]});

    instance.transferStar(accounts[1], tokenId);
    assert.equal(await instance.ownerOf(tokenId), accounts[1]);

```

Create a start and initiate the transfer, assert if the transfer is successful.

### Stars Tokens can be transferred from one address to another.



```
   let tokenId1 = 7;
        let tokenId2 = 8;
        await instance.createStar('Yet an other Star ', tokenId1, {from: accounts[0]});
        await instance.createStar('One more star', tokenId2, {from: accounts[1]});

        instance.exchangeStars(tokenId1, tokenId2);

        assert.equal(await instance.ownerOf(tokenId1), accounts[1]);
        assert.equal(await instance.ownerOf(tokenId2), accounts[0]);

```

Create 2 stars from two accounts and initate the exchange to 'assert' if the transfer is sucessful.


## Deploy your Contract to Rinkeby

It is deployed on the Rinkeby Ethereum Testnet with the following details:



```
  rinkeby: {
      provider: () => new HDWallet(mnemonic, `https://rinkeby.infura.io/v3/${infuraKey}`),
      network_id: 4,       
      gas: 4500000,   
      gasPrice: 10000000000
    }
```

Token Name: [**Star Token Udacity**](https://rinkeby.etherscan.io/token/0x1545772b309944d09b6e71c49bda7978447340ff)  
Token Symbol: **STU**  
Contract Address: [**0x1545772b309944d09b6e71c49bda7978447340ff**](https://rinkeby.etherscan.io/address/0x1545772b309944d09b6e71c49bda7978447340ff)


## Modify the front end of the DAPP to lookup the star

```
  const starName = await lookUptokenIdToStarInfo(id).call();
```
call the lookUptokenIdToStarInfo from the contract and update the label async.


## Token Details and Development tool versions

ERC 721 - Token Name: [**Star Token Udacity**](https://rinkeby.etherscan.io/token/0x1545772b309944d09b6e71c49bda7978447340ff)  
ERC 721 - Token Symbol: **STU**  
Truffle version : **v5.0.3**
OpenZeppelin: **V2.1.2**
