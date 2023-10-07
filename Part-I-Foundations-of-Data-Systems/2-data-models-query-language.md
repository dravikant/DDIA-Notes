### Introduction
Most applications are built by layering one data model on top of another
App code -> database -> bytes -> electrical current

app layer -> real world problem : classes and objects, APIs

storage layer -> json/xml, tables in rdbms or document in nosql db / graph 

db layer --> represent json/xml in bytes on disk

hardware --> bytes un terms of current

They are central to how problems are solved and thought about


SQL/No SQL

tables document structure

e.g. linkedin profile :

store as different table : user region industry position education
document : all in single doc

document model as better locality 



Relational model : schema is defined and data has to be matched at the time of insertion -- schema on write

nosql : no schema "unstructured data" but code assumes some structure -- schema on read 

### Object-Relational Mismatch

disconnect between objects in code and tables in db 

Impedance mismatch - Sometimes the structure of SQL might not match reality of application code

multiple joins might be required to pull data

Query optimizer decides query plan and indexes to use

People might have multiple previous/current jobs, but only stored in a single column in a structured way or require extra work to organize
Document oriented DB’s might alleviate some of these problems



no sql : no joins 

emulate joins in the code

difficult to refer to nested items

### Many-to-One and Many-to-Many Relationships


many one to many relationships : tree like structure --> nosql appears better

many to many -- graph


### Query language

declarative : tell only what to do --> sql



imperative : tell what to do and how to do 
--> hard to deal and parallelise



