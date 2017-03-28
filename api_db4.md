# Dictionary SQLite database format
## Views 2
The column `lang` is created by the views

### entry 3
col       |type|desc
----------|----|---------
id        |unique int| entry identifier
entry     |json| object representing one dictionary article
#### FIre 4
This document describes the SQLite export format of the "Bokmålsordboka og
##### Hei 5
Nynorskordboka" dictionary.  This is a database file containing a collection of
dictionary entries and supporting indexes.

The supporting tables should make it easy to locate entries by searching for  
words in various forms.  The `headword` and `inflection` views/tables can be  

###### Hei 6
deleted without loosing information, as this information can also be extracted
The following pragma declares this version of the database:

```
`pragma application_id = 0x6e626e6e`
`pragma user_version = 1`
```

