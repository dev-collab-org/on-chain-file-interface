The Nonagon Cup contract is the original motivation for this standardization effort.

It includes the HasFile interface defined for the [Nonagon Cup contract](https://github.com/ivyroot/nonagon-cup). This interface allows for a single file per token generated in varying size chunks. 


Some limitations of Nonagon Cup's HasFile to consider:

- it does not return content type to the client. Instead the first block holds the file format specific header bytes.
- it requires contracts to add a public method that returns a filename for each file. Each additional public method name seems to increase deployment gas costs a lot. 

