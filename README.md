# sqlleviate
Alleviates SQL use in Java projects by adding high-level API's for things like batch inserts, prepared statements and querying.


```java
QueryBuilder.Select select = QueryBuilder.selectAll();
select.from("accounts");

if (mustFilter)
    select.where("id", 9); // Optionally SQL.PARAM can be given to output a ? to use as prepared statement
    
if (sortMoney)
    select.order("money", richestFirst ? SQL.DESC : SQL.ASC); // Sort money
if (sortAge)
    select.order("birth_date", SQL.DESC); // If money is sorted, this is a sort added after sorting money.

if (notTooMany)
    select.limit(10);
    
String built = select.build();
```

^ Prototype :-)
