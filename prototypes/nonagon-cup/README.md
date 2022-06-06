
Implemented In Contract: Nonagon Cup ERC 721 NFT

Public Repo: [Nonagon Cup Github Repo](https://github.com/ivyroot/nonagon-cup)

Blockchain Explorer: [Nonagon Cup Etherscan](https://etherscan.io/address/0xa3a73cd8adf2a75c44185d1588d7d2d9f3f07544#code)


The Nonagon Cup contract is the original motivation for this standardization effort. It includes the HasFile interface defined for the [Nonagon Cup contract](https://github.com/ivyroot/nonagon-cup). This interface allows for a single file per token generated in varying size chunks. 


Some limitations of Nonagon Cup's HasFile to consider:

- it does not return content type to the client. Instead the first block holds the file format specific header bytes.
- it requires contracts to add a public method that returns a filename for each file. Each additional public method name seems to increase deployment gas costs a lot. 
- requires contracts to add a getFullFile public method which is duplicative of getFileChunk.
- requires contracts to know how many chunks there are via the getFileChunksTotal. it would be easier to implement on-chain file generation if the end condition can optionally be part of the generator function.
- extra deployment gas costs from requiring contracts to implement public methods getFullFile and getFileChunksTotal

