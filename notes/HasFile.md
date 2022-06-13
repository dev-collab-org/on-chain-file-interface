
Implemented in the Nonagon Cup ERC 721 NFT, the HasFile interface was created to allow STL files to be downloaded from contract. The interface supports a single file and filename for each token in the NFT. A simple javascript client which downloads these files can be tried out on the website file download pages, e.g. [nonagoncup.com/files/1](https://nonagoncup.com/files/1) 

Public Repo: [https://github.com/ivyroot/nonagon-cup](https://github.com/ivyroot/nonagon-cup)


Some limitations of Nonagon Cup's HasFile to consider:

- it does not return content type to the client. Instead the first block holds the file format specific header bytes.
- it requires contracts to add a public method that returns a filename for each file. Each additional public method name seems to increase deployment gas costs a lot. 
- requires contracts to add a getFullFile public method which is duplicative of getFileChunk.
- requires contracts to know how many chunks there are via the getFileChunksTotal. it would be easier to implement on-chain file generation if the end condition can optionally be part of the generator function.
- extra deployment gas costs from requiring contracts to implement public methods getFullFile and getFileChunksTotal

