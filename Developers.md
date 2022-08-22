
# Developers

## Vision
Before you check out the developer section, we recommend you check out [short video of what our vision](https://www.youtube.com/watch?v=HDIqYvR0has) on how we see liminal.market becoming a pipeline for multiple other projects.

## Early stage
This is early stage develpment documentation, so there are not much detail here. We encourage you to check out our [Discord channel](https://discord.gg/ePs5cRceNB) for discussion and to help us build up the documentation

## Development
As a developer, you are either reading data or writing data. You should know which one you are doing.

## Reading
When reading data, you are retrieve data that has already been written onto the blockchain and working with the data to display it in some way. This usually involves reading data from a JSON source. Liminal.market has already setup an [index on TheGraph](https://thegraph.com/hosted-service/subgraph/liminal-market/liminal-market), to simplify the process for you.

You can play with the data in [TheGraph playground](https://thegraph.com/hosted-service/subgraph/liminal-market/liminal-market). There you have ready made templates.

Here is an simple example of Javascript code that pull the data

    let  url = 'https://api.thegraph.com/subgraphs/name/liminal-market/liminal-market';
    
    let  query = { query:  'query { liminalMarketInfos { txCount, walletCount, value }}'};
    let  request = await  fetch(url, {
    	method:  'POST',
    	headers: { 'Content-Type':  'application/json' },
    	body:  JSON.stringify(query)
    });
    
    let  result = await  request.json();
    let  data = result.data.liminalMarketInfos[0];
      
    //data from response
    let  walletCount = data.walletCount;
    let  txCount = data.txCount;
    let  value = data.value;

We will provide more example and ready made libraries in the future.

## Writing (Buy & sell stock)
When writing data, you are executing smart contracts. You should be working with 3 smart contracts.

### Buy a symbol flow
When buying stock the flow is the following

1. Retrieve the address of the symbol on chain using LiminalMarket.getSecurityToken method
2. Exeucte transfer on aUSD token

### LiminalMarket - contract

- Mumbai address: 0x2BFb0207BC88BA9e2Ac74F19c9e88EdCcdBbC2a9

You need to retrieve the address of the symbol you want to buy. To do this you call the method getSecurityToken(symbol). You send in the symbol you want to buy (e.g. MSFT for Microsoft). The method will return the address of the ERC20 token that represents the stock.

### aUSD - contract

- Mumbai address: 0xe65Fb29C8CeB720755D456233c971DDb11fcbb8d

By calling the transfer method, you are buying a stock. You call the aUSD.transfer(recipent, amount). The recipent is the address of the symbol that you got from LiminalMarket.getSecurityToken(symbol)

### SecurityToken - contract
Address depends on which stock you are selling. It is only possible to sell stock that the user already own, so the contract address should be retrieved from the users wallet.

When you have retrieve the address, you can initate the contract as a standard ERC20 contract. You then call

    contract.transfer(recipient, amount)

The recipient is always the aUSD address (see above).
The amount is the quantity of shares you want to sell. This is NOT dollar amount.

