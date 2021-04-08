## SQL vs MongoDB Terminology comparasoin

The following table presents the various SQL terminology and concepts and the corresponding MongoDB terminology and concepts.


|SQL Terms | MongoDB Terms|
|-----|-----|
|Database| Database|
|Table|Collection|
|Row Record|Document|
|Column| Field|
|Index|Index|
|Table Join| $lookup, embedded document|
|Select into new table| $out|
|Merge into table| $merge|
|Union All| $unionWith|


# SQL vs MongoDB Query

## List available database

>SQL Version

    show databases

>MondbDB Version

    show dbs


## Select specific database

>SQL Version

    use {database name}

>MondbDB Version

    use {database name}


## Select specific database

>SQL Version

    use {database name}

>MondbDB Version

    use {database name}

>Show current database

    //SQL Version
    show database()

    //MongoDB
    db

    




|SQL|MongoDB|Description|
|----|----|----|
|```show databases```| ```show dbs```|List available database|
|```use {database name}```|```use {database name}```| Select specific database|
|```show database()```|```db```| show current database|
|drop {database name}|```use {database name} ```|Drop database|




