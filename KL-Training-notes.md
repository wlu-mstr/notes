0. KL: inut data
---------------------
### Batch Level
- Fastpath 1: scheduled/tide
- Fastpath 2: adhoc
- Fastpath 3: combine 1 and 2

### File Level
- NCOA: national change of address

### Record Level
- Country code:  either US or CA
- Local: EN or FR
- User_Full_Name_Flag
- User_Address_Last_Line_Flag

Platform
-----------------------
```
KL 3.5
-connected recognition-
- input data are from stage (pre processing)
- output data are for post processing
Hygiene	
Match                 	Management &
	|						      
	v					      	
Reference Base (RB)       Control System
						      	
```
1. Hygiene Process
-----------------------
### Pre-hygiene process:
- Performs up-front validations:
	- File
	- File counts
	- sequence number uniqueness assigns a date/time to the batch-every file associated with the batch will have the same prefix so they can be tied together:
		```<client>_<YYYYMMDD>_<HHMM>```
- Removes "bad" characters
- Seperates, sorts and segments all the data by contry 
	- US
	- CA
	- ZZ

###Country Specific Functions : US, CA
- address standardization, verification and parsing
	US-CASS, DPV, DSF2, LACS
	CA-SERP
- Invalid Addr Flag
	non-validated addr
	based on street addr/city values
	uses **merkle contry specific vulgar address tables**
- Create Street Name Match code
	compensates for some miss-spelling and transportation
- COA: Change of address
- Gonvernment ID Validation

###ZZ data
- no hygiene prrformed

Hygiene Process: COA
-----------------------
###change of address processing
- data is from vendor
- hit COA, then a new record is **created** (sequence number is not unique: a hard link between new an old record)

###Record with Old Address is standardlized and maintained
- KL_Create_Record_Flag = N

###Record with New Address is created
- All fields are the same except address fields AND
- KL_Created_Record_Flag = Y

###Both records with Old and New Address receive NCOA data
- US Address:
		return USPS NCPA Codes & USPS Address Components
- Canadian Address:
		retorn CPC NCOA Codes & CPC Address Components

#Hygiene Process: Address Examples
-----------------------
###Address Before Hygiene
```
Country code 		Address 				Address2 		City, state Zip
US 					123 main st 			Apt1 			Batimore, MD 1234
US 					123 main street 		Apt1			Batimore, MD 1234
US 					123 main street 		Apt # 1 		Batimore, MD 1234
US 					123 main strees Apt 1 					Batimore, MD 1234
```

###Address After Hygiene
```
Country code 		Address 				Address2 		City 		state 	Zip
US 					123 main street			Apt1 			Batimore 	MD 		1234
US 					123 main street 		Apt1			Batimore 	MD 		1234
US 					123 main street 		Apt1	 		Batimore 	MD 		1234
US 					123 main strees			Apt1 			Batimore 	MD 		1234
```

Hygiene Process: Organization Name Input/Output
-----------------------
noise Name 			-->  Standardize

Hygiene Process: Common Functions
-----------------------
- Phone validation
-	Date of birth validation
-	Email address validation and minor corrections
-	Hash key assignment
  - **critical concept when working with KL**
	-	Hash Key - A value generated for record *frome on all fields potentially used in any part of the match process*

Hygiene Process: Hygiene output files
-----------------------
- HASH
- PROPERCASE
- Control files
	- HYGCOMPLETE
	- PROPERCOMPLETE

2. Knowledge Link: Match Process
-----------------------
- HYGCOMPLETE and HASH output file as input of MP
- 2 tyoe of MP, Standard and Overlay:
	- Fastpath option 1 & 2 - standart MP
	- Fastpath option 3 - overlay 
### Drop Rules
- minumum requirement for a record to receive at least one / two / more keys
- keep reference base clean
- customizable at country level
	- Example: Name & address ... a rule

### Hash Match
** cretical concept when working with KL**
- HP create Hashkey on each HASH output record
- MP locate that hash key in a record previously loated to the reference base

### clusters
- **Cluster** - high level match rule designed to bucket groups of records together for a specific key type (Individual, Household, address, bussiness, custom)
	- subCluster: detailed match rule which defines the creation of a clustervalue for all records that meet criteria for inclusion in the cluster/subcluster
	- cluster value: text string at the subcluster level consisting of elements concatenated together to produce a string matching string
- Cluster with multi sbuclusters
	- A: Fist Name + Last Name Segment1 + Address
	- B: Fist Name + Last Name Segment2 + Address
		- Last name is Taylor jones, then it has two segmentations
		
### Anti-join
- **anti-join** - any field used to allow for matching between records with less information and records with more information, without creating an overmatch of 2/more records with conflicting information

- apply at cluster level
```
1	john	smith		123 main st, 12345	**100**
2	john	smith	SR	123 main st, 12345	**100**
3	john	smith	JR	123 main st, 12345	**101**
```
	- generation qualifier matters!!

## change of address
- coa matching requires a hard link between 2 records
- HP generates a 2nd record for every input record that has a *hit* from mover software (NCOA, CNCOA)
	- the 2nd record has same input_sequence_numbber and all other fields except address field.
- KL3 indentifies **this link via the input sequence number**:
```
xx	seq	first	last	addr			KL created rec	ID
1	123	john	smith	123 main st,1234	N		120
2	123	john	smith	456 Elm st, 67890	Y		120
```

- Antijoins are applied to COA clusters (configurable) to prevent over matching

3. (Post Processing) Output and Record
--------------------------------------
### Output files -> /mtch_out
- us/ca/zz base
- us/ca coa
- us/ca drops
- ID changes (1 file per entity tpye)
- MTCHCOMPLETE

### Report Files -> /mtch_rpt
- batch overview
- hygiene
- source overview
- source overlap
