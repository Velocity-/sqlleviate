# sqlleviate
Alleviates SQL use in Java projects by adding high-level API's for things like batch inserts, prepared statements and querying.

#### Query building with control statements
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

### Named prepared statements
```java
NamedStatement ns = QueryBuilder.named("SELECT * FROM accounts WHERE id=").p("id").s(" AND name ILIKE ").pstr("name");
ns.setString("name", myName);
ns.setInt("id", 123); // Order of SQL parameters is irrelevant
```

### Batch inserts with objects
```java
// Simple pojo
class Account {
    String name;
    String password;
    int money;
}

// Either a lambda or any class implementing BatchTranslator
SqlBatch<Account> batch = QueryBuilder.batch("INSERT INTO accounts (name,password,money) VALUES (?,?,?)", (stmt, acc) -> {
    stmt.setString(1, acc.name);
    stmt.setString(2, acc.password);
    stmt.setInt(3, acc.money);
}).build();

List<Account> accs = Arrays.asList(john, joe, kevy);
batch.execute(accs); // Or any other collection or array object.. or even individually.


// Alternatively...
class Account implements Batchable {
    // .. fields ommitted, see above classdef
    public void batch(PreparedStatement stmt) {
        stmt.setString(1, acc.name);
        stmt.setString(2, acc.password);
        stmt.setInt(3, acc.money);
    }
}

// This is only legal if the type implements Batchable as above
SqlBatch<Account> batch = QueryBuilder.batch("INSERT INTO accounts (name,password,money) VALUES (?,?,?)").build();
batch.execute(accs);
```

### SQL background workers with futures
```java
// Create a new worker that lets the user do the committing (turns autocommit off), 
// prints the SQL upon failure and retries 5 times.
SqlWorker worker = SqlBuilder.worker().option(WorkerOption.MANUAL_COMMIT).option(WorkerOption.PRINT_ON_FAIL).retries(5);
worker.feed("SELECT count(*) FROM accounts", set -> {
    logger.info("Number of accounts: {}.", set.getInt("count"));
});
```

^ Prototypes :-)
