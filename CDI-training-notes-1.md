CDI Core & KL Process basics
==================================
### CDI: act of identifying each customer/consumer and critical contact/marketing information related to them;
- Individual: cunstomer, consumer, person
- Address: where person lives or receives mails
- Household: association of individuals living/mailing at an addr (sam e last name and address)
- Bussiness: non-residential entity for business level marketing

WHY?
=================
- end goal: clean data, and knowledge of various entieies within the clients consumer/prospect data
- end up with: name and addr and other customer contact data in a highly **standardized** format that is in *ready to use from*
- each entity (name, address, household, business or other) will be assigned a UNIQUE and PERSISTENT numeric ID. (note: **entity is from DDD?  in DDD entity is always with a ID**)
- by indentifying the cunstomer and how to contact them, the business can market to them and maintain the relationship with that consumer.

KL
==================
one of the engines of CDI. Others include:

- CDG: IntelliDress
  - some proprietary change of address information
- Analytic-I
  - limited matching capability
- DDI
  - Digital data specific indentification
      - cookie data -> which data to unkonwn?

System Overview
===================
data are from client -> Hygiene Engine -> Matching Engine -> Knowledge Center

Hard key: client supplies to identify a person

Hygiene-Field Validation
- KL Dropped records are simply provided back to us in a seperated "drop" file and can be processed differently for different clients.
- drop rules can be customized based on specific client needs, and will differ from client to client

Hygiene-standardization/verification
==========================
address verification engines:
- CASS: coding accuracy support system
- DPV: delivery point validation
- LACS: Locatable address conversion system
- DSF2: Delivery sequence file
- SERP(Canadian): search evaluation and recognition program

Hygiene-address completion
==========================
- fill in the postal code based on addr city/state
- fill in the city/state based on the postal code


Hygiene-COA (Change of Address)
==========================
- NCOA: national change of address

KL - Other data
==========================
- phone number: 10 length, valid area codes, non-repetitive #, non-sequencial number values (pattern)
- date of birth: leap year consideration
- email address: email format
- government id (social security number): similar as phone #

Hygiene - Match-Code
==========================
for example:
- First name "John" can has match code of its first initial: J
- Last name "Ceasar" has match code of "CZ"
- ST name "Main ST"" has match code of "MS"

**This is useful to match field values that are sould-alike but not exactly the same**.


Matching
========================
- customizable based on client needs
- end goal: identify input records that belong to the same person, addr, household, or business
- if an input record is identified as the same person/addr, it is addigned a unique ID, that ID doesn't change and will be reassigned if the name/addr is sent to KL **again** from the same client.
- for that, KL maintains a reference base of all the *post* pygiene and standardization data along with the various matching data used in the process. This is referred to as the **"Client reference base" or "reference base"**


Hash matching
=============
hash match process allow KL to very quickly identify input records that are **complete** duplicates. So that **matching process will be skipped**:

If KL has already assigned a set of ID values to this identical data, it can quickly retrive that information from the reference base and apply it to the input record, **bypassing the more resource intensive cluster based matching process**.

Cluster matching
=================
Many of the input rows will be brand new or have slight variations in the data that do not allow the hash match process to assign ID value.

Thease records **must** go through the full cluster matching process. The cluster matching process uses specific combinations of input data fields to try to identify like individuals, households, addresses, etc.

Specific portions of data fields, and even soundex or "**sound-alike**" values can be used as part of the cluster matching process in order to accommodate things like common misspellings.

Match code (MC) can be used here to implement "sound-alike" fild values, such as fist name initial, last name MC, street MC, etc.

Transitive clousure
===================
if a ~ b  and b ~ c, then a ~ b ~ c

