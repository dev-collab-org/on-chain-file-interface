# Generative File Interface Overview


## Goal:

The goal of this repository is to craft an EIP for a standard smart contract interface to return files generated on-chain.


## Background:  

The trend of creating NFTs where the file which is the object of the token is generated partially or completely on-chain has resulted in a variety of methods of producing files from smart contracts. This repository is intended to craft a proposal that improves the ecosystem of on-chain file generation. 

By establishing a standard interface for fetching object files the interoperality and 3rd party client support of on-chain file generation can improve dramatically. For example, this will allow for future generative contracts to remix source files on-chain and generate new outputs, for files to be incorporated on-chain into larger works, and allows a whole new category of client applications to be built which access file resources directly from the blockchain.

## Summary:

There are at least two significant problems to address in progressing the art of producing on-chain files. 

### Issue 1, no standard way to fetch on-chain files from contracts
The first issue is the lack of a standard way to fetch the on-chain file from a contract which generates one. 

In some cases the contract exposes public methods to generate the file or files for an on-chain resource. These file names are unique to each contract, preventing a whole layer of client-side support for proliferating beyond a few specially made clients. 

In some cases the optional metadata extension to ERC-721 is used by returning the entire file as a base64 encoded value in one of the fields. This has significant limitations due to the overhead of base64 encoding plus the max gas per read limit described below.


### Issue 2, remove Ethereum max read gas limitation from on-chain file generation

The second issue is the limit in filesize imposed by the gas limit on read transactions. Currently around 9 million gas, this limit is a hard cap on the complexity and detail that can currently be created with on-chain file generation. 

By establishing a standard interface for fetching object files which uses chunks the per-call gas limits of clients can be removed as a bottleneck, allowing generation of arbitrarily large on-chain files.


## WEN ERC?

Here is the road from early discussions to ERC, your help is appreciated at any step!!!

Comment or submit PRs on this repo to help craft the proposal. Those who contribute will be credited as authors. 

Open feedback is solicited via github issues, submit one at any time for brownie points.

Create a contract using these techniques and reference its code to become an early implementer. Early implementer contracts will be featured in a listing  somewhere.

Once the proposal document (name TK) reaches a proposable state it can progress to the evaluation & support phase. Support will be solicited via a signable in-support-of-this doc somewhere and some experimental contracts will be published to exhibit the technology.

When sufficient support is generated an EIP will be generated and support for the proposal will move to the discussion on the forum post.
At this phase projects which implement the proposal can describe themselves via the EIP number. 

It will take a community, but implementers of ERC-721 and ERC-1155 with on-chain components will be sure to benefit from a world where an associated file standard exists. 


## References: 


Original motivation for this standardization effort came from the HasFile interface defined for the [Nonagon Cup contract](https://etherscan.io/address/0xa3a73cd8adf2a75c44185d1588d7d2d9f3f07544#code#F6#L1). This interface allows for a single file per token generated in varying size chunks. It also includes generation of a file name. 

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

