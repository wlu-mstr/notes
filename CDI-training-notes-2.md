Matching: ANTI-join
====================
use certain key fields to determine if two otherwise apparently equal records obviously should not be matched together. Common anti-join fields are:
- standardized generation qualifier (JR, SR, III)
- gender
- government id (ssn) 
- first name (for first initial clusters)
- confirmed exact street name (for address clusters)
- middle initial
- client hard keys

Matching: Over/Under matching
=============================
- Over matching: identifying input records with the same ID values (Individual, address, household, etc), but when reviewed by another person, may not be a good match;
- Under matching: identifying input records with differing ID values (...) that when reviewed by another person, may appear to be the same.

      ```Pretty similar as over/under fitting in mathmatics.```


How to implement the common KL CDI process as part of a KC marketing database
============================================================================
- preparing staged data from input data to KL
- Creating the KL input file
- KL processing concerns
- Understanding KL output data
- Preparing KL staging process
- CDI "Besting" process
- CDI core data model

