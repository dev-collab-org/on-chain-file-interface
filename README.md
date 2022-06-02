# Generative File Interface Overview


## Goal:

The goal of this repository is to craft an EIP for a standard smart contract interface to return files generated on-chain.


## Background:  

The popularity of on-chain NFTs has resulted in many non-standard interfaces for fetching files from smart contracts and one non-optimal standard interface (base64 json as data tokenUri). This repository is intended to craft an interface that improves the ecosystem of on-chain file generation by standardizing it and enabling file generation to be done in chunks. Note that the interface is not only for NFTs, ERC 721, or ERC 1155 or any other existing standard. It can be used in any type of contract. 

## Benefits:

Widespread adoption of a standard would lead to a sudden leap in the capabilities of the on-chain file generation community. For example, this will allow for future generative contracts to remix files from other on-chain contracts and generate new outputs. A chunked interface will also allow generation of large files on-chain.

## Summary:

There are at least two significant problems to address in progressing the art of producing on-chain files. 

### Issue 1, no standard way to fetch on-chain files from contracts

The first issue is the lack of a standard way to fetch the on-chain file from a contract which generates one. 

In some cases the contract exposes public methods to generate the file or files for an on-chain resource. These file names are unique to each contract, preventing a whole layer of client-side support from proliferating beyond the few clients made specifically for a smart contract.

In some cases the optional metadata extension to ERC-721 is used by returning the entire file as a value in one of the fields of a base 64 encoded JSON data URI. This has significant limitations due to the overhead of base64 encoding plus the max gas per read limit described below.


### Issue 2, remove Ethereum max read gas limitation from on-chain file generation

The second issue is the limit in filesize imposed by the gas limit on read transactions. Currently around 9 million gas, this limit is a hard cap on the complexity and detail that can currently be created with on-chain file generation. 

By establishing a standard interface for fetching object files which uses chunks the per-call gas limits of clients can be removed as a bottleneck, allowing generation of arbitrarily large on-chain files.


## Roadmap

### Stage 1: create new interfaces and try using them

To start multiple devs will come up with interfaces that work for specific contracts they are familiar with. In some cases interfaces are already deployed that fit this description with only slight modification. In other cases the interfaces will be part of new contracts or just standalone sketches. 

Participating devs will add their interfaces to this repo via PR. 


### Stage 2: review and merge

Participating devs will discuss the pros and cons of the various interfaces that have been submitted and reach a consensus on what is best for a standard. The final version will likely take elements from a few submitted interfaces. This stage will include discussion on the Ethereum Wizards forum.

### Stage 3: pre-ERC implementations 

Other devs will be solicited to integrate the standard into their contracts before the acceptance of the ERC. This makes sense for contracts that were going to expose custom file access methods anyway. 


### Stage 4: ERC acceptance

Once a critical mass of contracts are using the standard interface the EIP will likely be accepted and get an ERC number. Every one who has commited to the github will be listed as a co-author of the interface. 

It will take a community, but implementers of ERC-721 and ERC-1155 with on-chain components will be sure to benefit from a world where an associated file standard exists. 


## References: 


Motivation for this standardization effort came from the HasFile interface defined for the [Nonagon Cup contract](https://etherscan.io/address/0xa3a73cd8adf2a75c44185d1588d7d2d9f3f07544#code#F6#L1). This interface allows for a single file per token generated in varying size chunks. It also includes generation of a file name. 

```
/**
 * @dev Fully On-Chain File Access Interface -- Single file per token variant
 * Interface for contracts which expose a single file for each of their tokens.
 * Originally created for the NonagonCup.
 */
 interface HasFile {

   /**
    * @dev Each file must have a filename including the file extension, for example:  "my-file-1.stl"
    * If the contract is an NFT where the object of the NFT is the exposed files then the filenames
    * within the contract must be unique and must include the tokenId that they belong to.
    */
   function getFilename(uint256 tokenId) external view returns(string memory filename);

   /**
    * @dev Single call to get the full file contents.
    * NB: This may not work in all conditions due to gas limits, it is instead
    * recommended to call getFileChunksTotal and iterate calls to getFileChunk.
    */
   function getFullFile(uint256 tokenId) external view returns(bytes memory fileContents);


   /**
    * @dev Each file has zero or more fileChunks.
    */
   function getFileChunksTotal(uint256 tokenId) external view returns(uint256 count);


   /**
    * @dev Use repeated calls to getFileChunk to get the binary data for the file.
    *  -- Size of returned data for each fileChunk must be no more than 1024 bytes.
    *  -- First fileChunk should be the file header, if the file format has one.
    */
   function getFileChunk(uint256 tokenId, uint256 index) external view returns(bytes memory data);

 }
```

