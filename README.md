# AccessHistory
This is a (not too) poor man's framework to add system versioning to Microsoft Access based databse projects.

## What is this need for?
Did you ever wish you could track the history of your database entries in order to be able to reproduce the state of the facts stored in your database for any  given point in time (e.g. to reproduce an invoice with articles that changed meanwhile)? A straight forward approach is to simply duplicate all the facts instead of referencing them. For the example of the invoice you would copy all the attributes of the each product from the article's table to the invoice position record in order to conserve them.

But hey, that's how you lose the advantage of relational databases right? A more elegant answer to this need is the concept of a [temporal database](https://en.wikipedia.org/wiki/Temporal_database) (see also "Inner workings" for a brief jumpstart explanation). Temporal features have been introduced in standard SQL with [SQL:2011](https://en.wikipedia.org/wiki/SQL:2011) (sometimes called "system-versioning), however only a few commerical systems such as e.g. [IBM's DB2](https://www.ibm.com/developerworks/data/library/techarticle/dm-1204db2temporaldata/), [Oracle's DB 12c](https://oracle-base.com/articles/12c/temporal-validity-12cr1) or [Microsoft's SQL Server 2016](https://msdn.microsoft.com/en-us/library/mt590957.aspx) are supporting these features. And so far, there is no support for temporal features by Microsoft Access in sight.

### What this project does
This project provieds a (not too) poor man's approach to replicate some of the the temporal features and behaviour in Microsoft Access. The main goal is to take all the boring, repetetive and error-prone tasks needed to introduce the time dimension into the database off the shoulders of the database developer. The scripts published provide:
* Macros automatically adding timespan columns to all tables
* Macros adding and updating all the data macros needed (see "Inner workings" section) in order to keep track of the database's history
* Helper functions to simplify creation of "temporal queries"

### What this project doesn't (Limitations)
* It doesn't provide an enhancement to the Access Database Engine engine. Therefor it doesn't provide a high performance solution. On the other hand if performance really matters, maybe the Access Database Engine isn't the best pick anyway...
* This project is strictly bound to an Microsoft Access backend. For all other backends (e.g. MS SQL, ODBC, etc.) different solutions are needed.

## How to use it
### Requirements
* The tool makes heavy use of the MS Access version of Triggers called ["data macro"](https://support.office.com/en-us/article/Create-a-data-macro-b1b94bca-4f17-47ad-a66d-f296ef834200) introduced with (IIRC) Microsoft Access 2010.
* The tool has been developed and tested using Microsoft Access 2016. Reports for other compatible versions are highly welcomed. 

### Designing temporal queries
In order to understand how to desing temporal queries, a brief understanding of temporal databases as implemented by this project is needed. Therefor reading the section "What is a temporal database?" in the "Inner workings"-chapter is highly recommended.

## Inner workings
### What is a temporal database?
Temproal databases add a timeline to the facts (records) stored in your database by augmenting the primary keys of your records with a validity timespan. If you assume a primary key being a unique integer number, this number is now extended by a timestamp designating the first moment where the record in this form has been known (*ValidFrom*) and the first moment where the record in this form has been superseeded (*ValidUntil*). What makes the primary key unique now is not only the integer number but the number at a given point in time. Or to put it differently: No two records with the same number must ever have overlapping validity timespans.

There are reasons why this is not enough to cover all cases and therefor a [bitemporal approach](https://en.wikipedia.org/wiki/Bitemporal_Modeling) is needed. However for now this project focusses on the the pure system-versioning approach.

