# On-Chain File Interface Overview


## Goal:

The goal of this repository is to craft an EIP for a standard smart contract interface to return files from EVM smart contracts.


## Background:  

The popularity of on-chain NFTs has resulted in many non-standard interfaces for fetching files from smart contracts and one non-optimal standard interface (base64 json as data tokenUri). This repository is intended to craft an interface that improves the ecosystem of on-chain files by standardizing access and enabling file generation to be done in chunks. Note that the interface is not only for NFTs, ERC 721, ERC 1155, or any other existing standard. It can be used in any type of contract. 

## Benefits:

Widespread adoption of a standard would create a significant leap in the capabilities of the on-chain file ecosystem. For example, this will allow for future generative contracts to remix files from other on-chain contracts. A chunked interface will also allow for arbitrarily large on-chain files.

## Summary:

There are at least two significant problems to address in progressing the art of on-chain files. 

### Issue 1, no standard way to fetch on-chain files from contracts

The first issue is the lack of a standard way to fetch the on-chain file from a contract which supports them. 

In some cases the contract exposes public methods to generate the file or files for an on-chain resource. These method signatures are unique to each contract, preventing a whole layer of client-side support from proliferating beyond the few clients made for one specific smart contract.

In some cases the optional metadata extension to ERC-721 is used by returning the entire file as a value in one of the fields of a base 64 encoded JSON data URI. This has significant limitations due to the overhead of base64 encoding plus the max gas per read limit described below.


### Issue 2, remove Ethereum max read gas limitation from on-chain file generation

The second issue is the limit in filesize imposed by the gas limit on read transactions. Currently around 9 million gas, this limit is a hard cap on the complexity and detail that can currently be created with on-chain file generation. 

By establishing a standard interface for fetching object files which uses chunks the per-call gas limits of clients can be removed as a bottleneck, allowing generation of arbitrarily large on-chain files.


## Roadmap

### Stage 1: Make Multiple Prototypes

#### devs create different interfaces from scratch for new or existing contracts

To start multiple devs will come up with interfaces that work for specific contracts they are familiar with. Actually using an interface is important to detect friction points that are not obvious when designing it. In some cases public methods are already deployed that can be turned into interfaces with only slight modification. In other cases prototype interfaces will be part of new contracts or just standalone sketches. 

Participating devs will add their interfaces to this repo via PR via the steps describe in the [prototypes](./prototypes/DEV.md) directory. All prototypes are intended to be experiments which will help create the standard interface but will not conform to it.


### Stage 2: Review Prototypes

Participating devs will try out the various interfaces that have been submitted, discuss the pros and cons of them, and reach a consensus on what is best for a standard. The final version will likely take elements from a few submitted interfaces. This stage will include discussion with existing EIP/ERC community on the Ethereum Wizards forum.


### Stage 3: Pick Final Version 

Once review has covered enough variations and questions to have a standard that seems likely to be a solid improvement for the Solidity coding community, a final version will be locked in place. Every one who has commited to the repo hosted on GitHub prior to the final version pick will be listed as a co-author of the interface.

After a final version is crafted, devs will be solicited to integrate the standard into their contracts before the acceptance of the EIP into an ERC. This makes sense for contracts that were going to expose custom file access methods anyway since they get incremental benefits from a growing ecosystem of client support. 


### Stage 4: ERC acceptance

Once a critical mass of contracts are using the standard interface the EIP will likely be accepted and get ERC status. It will take a community, but those who like on-chain files will be sure to benefit from a world where an associated file access standard exists. 

