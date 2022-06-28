## 4 Step dimensional process

1. Gather business requirement
2. Declare grain
3. Declare facts
4. Declare dimensions.

## Basic fact table techniques

1. Fact table contains numeric data to be measured.
2. Additive can be summed across all records , semi-additive is which can be summed across partially for example account balance and non-additive which cannot be summed such as ratios etc.
3. NULLS must be avoided in fact table rather a surrogate key needs to be generated for dimension keys with attributes as unknown. 
4. Conformed facts so that they are identically named. 
5. Transaction fact table contains rows only if a transaction takes place hence they are sparse. They contain exact details like time stamps etc.
6. Periodic snapshot are records on periodic interval such as days, week, month and year.
7. Accumulating snapshot fact table consists of steps at a pipeline and after completion of each step records gets updated.
8. Factless fact tables are there to capture events such as promotion coverage because they wont be available in sales table. This type of table also has dummy value of 1 for easy calculation.
9. Aggregated fact tables are rollups of numeric fact data so accelerate performance at UI level , also known as aggregate navigation. 
They also contain foreign keys for shrunken dimensions.
10. Consolidated facts are those to merge those facts with same grain. 

## Basic Dimension table techniques

1. Every dimension table has a unique primary key and contains descriptive columns in dimension table with its associated primary key in fact table as foreign key.
2. Dimension surrogate keys are created because for one natural key there will be more than one record as we keep track of history. Also since natural key can be created by more than one source system they may be poorly administered.
3. Natural keys can be changed hence to identify a particular dimension DW/BI sytem can have their own durable key which never changes.
4. Drilling down means adding more column in GROUP BY clause so that additional details are gained.
5. Degenerate dimensions are used where multiple inline items are purchased with different invoice numbers and they have dont have their separate dimension table. 
6. Denormalized dimension table by reducing number of tables and adding columns in dimension table for simplicity and speed. 
7. Multiple hierarchies in same dimension table is similar to above point where instead of separate table we keep all attributes in same table. 
8. Abbreviations with code values should be supplemented with full text words in dimension attributes. 
9. Null values should be replaced with UNKNOWN or NA values to keep it independent of underlying database. 
10. For calendar date dimensions keep it as YYYYMMDD , so that it can be partitioned and also all other functions such as to_Date should be avoided instead use filter in date dimensions . 
11. One fact table may refer to same dimension table but with different logical dimensions such as date dimension but grouped by either week or month. This is called role playing dimension and each of them should have a separate column names to distinguish between them. 
12. Transaction profile dimension or JUNK dimension are those table which contain attributes with low cardinality flags and store each combination in dimension . No need to store each possible combination , only those who have come up till now. 
13. Snowflake dimensions create a multilevel structure due to normalization which can negatively impact query performance. 
14. Outtrigger dimension such as country demograhic in a customer table can be allowed because it improves significant space. 
15. Conformed dimension means columns in separate dimension table should have same name so that drilling across is easier. 
16. Shrunken dimensions only contain a subset of rows and columns and are primarliy used with aggregated facts. 
17. Drilling across simply means firing two seperate queries using fact table so that they can be merged with conformed dimensions. 
18. Value chain can be identified with natural flow of an organization. 

SCD Type 0:
Keep original and dont change anything

SCD Type 1:
Overwrite or update original value. 

SCD Type 2:
This maintains history by creating 3 new columns such effective , expiration date and current indicator. 

SCD Type 3:
Add an additional column to store past history.

SCD Type 4:
Create a mini dimension if a group of attributes change rapidly and then they are split off to a mini dimension. Primary key from both dimensions are put into fact table. 

SCD Type 5:
A separate view for current data in a dimension. (scd 4 + scd 1)

SCD Type 6:
Keep SCD type 2 but keep another column for current data like scd 3. 

SCD Type 7:
Same as SCD 5 but dimension is joined directly with FACT table.

## Dealing with Positional Hierarchies

1. Fixed Depth Positional Hierarchies :- Use additional attributes in dimension table for positional hierarchies. 
2. Slightly Ragged/Variable Depth Hierarchies :- Introduce a few more attributes to make it fixed depth. 
3. Ragged Depth Hierarchies :- Use of bridge table for all nodes . Sometimes pathstring can be used with overhead of updating all values as well.

## Advanced Fact table techniques

1. Fact table surrogate keys could be useful but not mandatory.
2. Centipede fact tables result when low cardinal foreign keys are used in fact tables instead of junk dimension.
3. If numeric values are aggregated then use them in fact if they are to be filtered then use them in dimensions.
4. Accumulating snapshots should store just one lag between each step so rest of them can be calculated.
5. In header/line or parent child all dimension foreign keys in header should be there in line as well.
6. Allocated facts later
7. Profit and loss fact Tables using Allocations
8. Multiple currency store one value in original currency and another in standard currency.
9. Multiple units of measure.
10. YTD should be handled at BI layer.
11. Never join two fact tables instead fire two separate queries and sort merge them later.
12. SCD Type 2 in fact table.
13. Late arriving facts if most current dimensional context does not match incoming row.

## Advanced Dimension table techniques

1. Dimension to dimension table joins should be avoided instead place foreign key in fact table.
2. Multivalued Dimension and bridge tables for example a patient diagnosed with multiple ailments so we create a brdige for ailments.
3. We can also apply SCD 2 in multivalued dimension.
4. Behaviour tag time series
5. Behaviour study groups
6. Aggregated facts as dimension attributes such as filtering on all customers who spent over a certain amount.
7. Dynamic Value bands is putting varying size range in a dimension.
8. Comments should be stored in dimensions rather than fact table. 
9. Multiple time zones should be stored in fact table.
10. Measure type dimensions
11. Step dimension is used at determine current step and how many steps are left.
12. Hot swappable dimensions.
13. Abstract generic dimension should be avoided in dimensional modeling.
14. Audit dimensions are metadata columns like insert/update time.
15. Late arriving dimensions means dimensional context has not arrived and these dimension row's has attributes that are specified as UNKNOWN. SCD Type 1 changes are done when dimensional context arrives.

## Special Purpose Schemas

1. In case of heterogenoeous products , build a supertype and subtype fact table.
2. Real time fact tables
3. Error event schemas where each error is the grain.
