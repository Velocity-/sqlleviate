# sqlleviate
Alleviates SQL use in Java projects by adding high-level API's for things like batch inserts, prepared statements and querying.


```java
QueryBuilder.Select select = QueryBuilder.selectAll("accounts");

if (mustFilter)
    select.where("id", 9); // Leaving out the value results in a ? for prepared statements.
    
if (sortMoney)
    select.order("money", richestFirst ? DESC : ASC); // Sort money (enum Sorting.DESC / ASC)
if (sortAge)
    select.order("birth_date", DESC); // If money is sorted, this is a sort added after sorting money.

if (notTooMany)
    select.limit(10);
    
String built = select.build();
```

^ Prototype :-)
