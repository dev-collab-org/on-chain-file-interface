Project Name: Nonagon Cup 

Website: [https://nonagoncup.com](https://nonagoncup.com)

Etherscan: [https://etherscan.io/address/0xa3a73cd8adf2a75c44185d1588d7d2d9f3f07544#code](https://etherscan.io/address/0xa3a73cd8adf2a75c44185d1588d7d2d9f3f07544#code)


The Nonagon Cup NFT contract was the original inspiration for this standardization effort. It includes an interface for fetching STL files: HasFile. This interface implements chunked (ie sliced/paginated) delivery of file contents to workaround gas limitations. 

The interface is described in [notes/HasFile.md](../notes/HasFile.md)

Client: The Nonagon Cup website includes a simple javascript client which uses the web3.js library to call the methods required to download files from the contract. The javascript then assembles the chunks into a single file in memory and downloads that. File download for each token is available on that token's file page, for example see the first one here:  [nonagoncup.com/files/1](https://nonagoncup.com/files/1) 

An example javascript file downloading client for the Nonagon Cup can be viewed here: [https://nonagoncup.com/js/app/download.js](https://nonagoncup.com/js/app/download.js)