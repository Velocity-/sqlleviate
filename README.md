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

```java
NamedStatement ns = QueryBuilder.named("SELECT * FROM accounts WHERE id=").p("id").s(" AND name ILIKE ").pstr("name");
ns.setString("name", myName);
ns.setInt("id", 123); // Order of SQL parameters is irrelevant
```

```java
class Account {
    String name;
    String password;
    int money;
}

SqlBatch<Account> batch = QueryBuilder.batch("INSERT INTO accounts (name,password,money) VALUES (?,?,?)", (stmt, acc) -> {
    stmt.setString(1, acc.name);
    stmt.setString(2, acc.password);
    stmt.setInt(3, acc.money);
}).build();

List<Account> accs = Arrays.asList(john, joe, kevy);
batch.execute(accs); // Or any other collection or array object.. or even individually.
```


^ Prototypes :-)
