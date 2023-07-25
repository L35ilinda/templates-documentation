# SQL Naming Conventions

## Object Naming Conventions
The naming conventions apply to all user defined objects.

The following general rules must apply:
- Object names must be in singular form.
- Object names must be in Pascal Casing E.g. ProductTable.
- Special characters are not allowed.

### Exceptions:
Underscores (_) may be used in:

- Intermediary table (tables that resolve many-to-many relationships) names. I.e. TableName1_TableName2
- Stored Procedures I.e. spi_StoredProcedureName
- Index Names I.e. IDX_TableName_IndexName

### Stored Procedure Naming Conventions:
All stored procedures must be prefixed with one of the following:

| Prefix | Denotation |Description|
| ----------- | ----------- | ----------- |
| sps_ | Stored Procedure SELECT |Stored procedures that only SELECT data from tables – no modifications to any table is allowed.|
| spu_ | Stored Procedure UPDATE |Stored procedures that UPDATE data only. |
| spd_ | Stored Procedure DELETE |Stored procedures that DELETE data only. |
| spi_ | Stored Procedure INSERT |Stored procedures that INSERT data only. |
| spm_ | Stored Procedure MERGE  |Stored procedures that MERGE data (INSERT/UPDATE/DELETE). They can contain SELECT statements.|

### Index Naming Conventions:
- Prefix with IDX_

Index names must be in the following format:

| Index Category | Naming Convention |
| ----------- | ----------- |
| Non-primary Key Indexes | IDX_TableName_SequenceNo * | 
| Primary Key Indexes | PK_TableName | 

\* Use the next number in the sequence. The first Non-Primary key index will be called IDX_TableName_01, the next index will be IDX_TableName_02 - etc.

### Foreign Key Naming Conventions:
Prefix with FK_
Reference the 1st table and the 2nd table I.e. TableName1_TableName2
TableName1 = the table on which the foreign key is created on, TableName2 = the table which is being referenced.

I.e. FK_TableName1_TableName2

### Defaults Naming Conventions:
Prefix with DF_
Reference TableName, underscore (_) and FieldName
I.e. DF_TableName_FieldName

## Casing

All **keywords** must be in **CAPS**:
```
IF OBJECT_ID(‘ProductDocs’, ‘U’) IS NOT NULL
DROP TABLE dbo.ProductDocs
GO
CREATE TABLE dbo.ProductDocs
(
          DocID INT NOT NULL IDENTITY
         ,DocTitle NVARCHAR(50) NOT NULL
         ,DocFileName NVARCHAR(400) NOT NULL
)
GO
```
## Object References

- All tables/views/stored procedures should be fully qualified when referenced in code, even if the default schema is dbo:
    ```
    SELECT
            c.ContractId
            ,c.ContractReference
    FROM    dbo.Contract c    
    WHERE   c.ContractId        = 123
    AND     c.SystemStatusDesc  = ''
    ```

- **Never use the (*) wildcard in your SELECT statements** – always specify the column names that you need, AND ONLY the columns that you’re going to use – NOTHING more!

    !NOT ALLOWED!
    ```
    SELECT * FROM Contract
    ```
- Always specify the columns in when using INSERT/SELECT INTO statements!
   ```
    INSERT INTO dbo.Contract
    (
                Field1
                ,Field2
                ,Field3
    )
    SELECT
                src.Field1
                ,src.Field2
                ,src.Field3
    FROM    dbo.SourceTable src
   ```

## Aliases

Keep them short, but clear. For example don’t use something like a, b or c as an alias. Rather use an abbreviation of the table name.
Always use aliases in your code.
See example above.

## Commas

- Commas (,) must be placed at the beginning of table columns:
   ```
    INSERT INTO dbo.Contract
    (
        Field1
        ,Field2
        ,Field3
    )
    SELECT
            src.Field1
            ,src.Field2
            ,src.Field3
    FROM   dbo.SourceTable src
    ORDER BY Field1
            ,Field2
   ```

## Spacing and Aligning

- SELECT / UPDATES – each field in the select list must be in its own line.

    ```
    INSERT INTO dbo.Contract
    (
                Field1
                ,Field2
                ,Field3
    )
    SELECT
                src.Field1
                ,src.Field2
                ,src.Field3
    FROM        dbo.SourceTable    src
    ORDER BY    Field1
                        ,Field2
   ```
- WHERE and ORDER BY clauses – each clause must be in on its own line. Make sure about clause sets.
    ```
    WHERE    (dwc.Field1 IS NULL AND dwc.Field2 = '') -- CLAUSE 1
    OR       (dwc.Field2 IS NULL)                     -- CLAUSE 2
   ```

## Data Types

Use Unicode data types for all character data. In other words NVARCHAR, NCHAR.

- Always specify the narrowest columns you can:

    The narrower the column, the less amount of data SQL Server has to store, and the faster SQL Server is able to read and write data. In addition, if any sorts need to be performed on the column, the narrower the column, the faster the sort will be.

    E.g. Do you really need that BIGINT? The INT data type can store up to 2,147,483,647.
    BIGINT is using 8 bytes – INT only 4 Bytes.

- If you need to store large strings of data, and they are less than 8,000 characters, use a VARCHAR data type instead of a TEXT data type. TEXT data types have extra overhead that drag down performance. If the amount of data is more than 8,000 characters – use VARCHAR(MAX)

- If the text data in a column varies greatly in length:

    Use a VARCHAR data type instead of a CHAR data type. The amount of space saved by using VARCHAR over CHAR on variable length columns can greatly reduce I/O reads cache memory used to hold data, improving overall SQL Server performance. But performance can also be increased if the data length is consistent to use CHAR

- If you have a column that is designed to hold only numbers, use a numeric data type:

    Such as INTEGER, instead of a VARCHAR or CHAR data type. Numeric data types generally require less space to hold the same numeric value as does a character data type. This helps to reduce the size of the columns, and can boost performance when the columns is searched (WHERE clause), joined to another column, or sorted.

- Money or Decimal?:

    For any field that stores “currency” data – use Money, else use DECIMAL.

- Float and real data types are not allowed:

    Only use the DECIMAL data type.

## Comments

- Comments go a long way!
- Utilise (copy and paste) the below template into objects (Views / Stored Procedures / SQL Scripts / etc.) as object headers:

## Other
- Use SET NOCOUNT ON at the beginning of SQL batches and stored procedures.
This will suppress messages like (1 rows affected). This will improve performance and network traffic will be reduced.
- All tables must have a Unique Clustered Primary Key defined
-Avoid loops/cursors where possible. 99% of the time a query can be changed into not using loops/Cursors
- Do not use reserved words for naming ANY object in SQL.
Reference: http://msdn.microsoft.com/en-us/library/ms189822.aspx


# Data Warehouse Object Naming Conventions
 Follow the same naming convention rules as mentioned in the SQL Server Naming Conventions section above.

## Schema Objects

|Schema|Denotation|Description|
|---|---|---|
|dw|Data Warehouse|This schema segregates the data warehouse operational related objects (config tables / log tables / error event schema / etc.) that reside within the relational data warehouse database.|
|dm|Dimensional Models|This schema segregates the dimensional model tables (dimensions & facts). It resides within the relational data warehouse database and is the secondary presentation layer for the data warehouse. These are the final dimensionally modelled tables. No staging data / temp tables / etc. reside within this schema.|
|vFct|View Fact|This schema segregates the dimensional model fact views. It resides within the relational data warehouse database and is the primary presentation layer for the data warehouse. These views expose the fact tables that reside within the "dm" schema.|
|vDim|View Dimension|This schema segregates the dimensional model dimension views. It resides within the relational data warehouse database and is the primary presentation layer for the data warehouse. These views expose the dimension tables that reside within the "dm" schema.|
|dme|Dimensional Model Extracts|This schema segregates the dimensional model extract related objects (extract stored procedures & tables / etc. (perm / temp)) that reside within the relational data warehouse database.|
|dws|Data Warehouse Staging|This schema segregates the database objects (stage tables / stored procedures / etc. - excluding views) that reside within the data warehouse staging database.|
|dwsv|Data Warehouse Staging Views|This schema segregates the views that reside within the data warehouse staging database. These views expose the the data from the database objects that reside within the data warehouse staging database.|
|rpt|Reports|This schema segregates the dimensional model / operational reporting / dashboard related objects (stored procedures / tables / views / etc. (E.g. rpt.usp_Sales / rpt.Sales (Table) / etc.) that reside within the relational data warehouse database. All reporting based database objects, whether they are exposing data from the dimensional models or not (data loaded directly from the source systems into a table that is not part of a dimensional model (E.g. the report matrix dashboard data)) reside here. I.e. only report specific database objects reside here.|

## Tables

Fact Tables:

Types:

There 3 types of standard fact tables and 1 assisting type fact table within Dimensional Modelling:

|Type|Acrynym|Description|
|---|---|---|
|Transactional Fact|Tx|A fact table that has a Transactional grain of data within the fact table.|
|Periodic Snapshot Fact|Ps|A fact table that has a periodic snapshot (data loaded at a specified period) grain of data within the fact table.|
|Accumulative Snapshot Fact	|As|A fact table that has an accumulative (milestone / pipeline  based) grain of data. I.e. 1 record per pipeline whereby the relevant milestone columns are updated whenever the relevant milestone has been reached.|
|Aggregated Fact|Ag|This type of fact table compliments / supports / assists 1 of the other types of fact tables by aggregating the data to a higher level. These types of fact tables are normally created to aid processing time for OLAP models or reports etc.|

Naming Rules:
- Reside within the "dm" schema.
- Must represent the business process that is being modelled E.g.:

|Business Process| Fact Table|
|----------------|-----------|
|Instruction SLA|dm.FctTxInstructionSLA|
|Instruction Rejection|dm.FctTxInstructionRejection|
- Must be in the singular form. E.g.: 

    dm.FctTxTransaction NOT dm.FctTxTransactions

    dm.FctTxInstructionSla NOT dm.FctTxInstructionsSla

- Prefix the table with "Fct" and the acronym of the type of fact table E.g.:
    dm.FctPsCurrentAum
    dm.FctTxTransaction

Dimension Tables:

Naming Rules:
- Reside within the "dm" schema.
- Must be self-explanatory.
- Must not be ambiguous.
- Must be in singular form E.g.:

    dm.DimFinancialAdvisor NOT dm.DimFinancialAdvisor**s**

- Prefix the table with "Dim" E.g.:
    
    dm.DimInstructionType