# Materialized View Strategies Using PostgreSQL

Queries returning aggregate, summary, and computed data are frequently used in application development. Sometimes these queries are not fast enough. Caching query results using Memcached or Redis is a common approach for resolving these performance issues. However, these bring their own challenges. Before reaching for an external tool it is worth examining what techniques PostgreSQL offers for caching query results.

### Example Domain

We will examine different approaches using the sample domain of a simplified account system. Accounts can have many transactions. Transactions can be recorded ahead of time and only take effect at post time. e.g. A debit that is effective on March 9 can be entered on March 1. The summary data we need is account balance.

```postgresql
create table accounts(
  name varchar primary key
);

create table transactions(
  id serial primary key,
  name varchar not null references accounts
    on update cascade
    on delete cascade,
  amount numeric(9,2) not null,
  post_time timestamptz not null
);

create index on transactions (name);
create index on transactions (post_time)
```

### Sample Data and Queries

For this example, we will create 30,000 accounts with an average of 50 transactions each.

All the sample code and data is available on Github.

Our query that we will optimize for is finding the balance of accounts. To start we will create a view that finds balances for all accounts. A PostgreSQL view is a saved query. Once created, selecting from a view is exactly the same as selecting from the original query, i.e. it reruns the query each time.
me.


### PostgreSQL Materialized Views

The simplest way to improve performance is to use a materialized view. A materialized view is a snapshot of a query saved into a table.


### Sources

[https://github.com/jackc/mat-view-strat-pg]

[https://www.postgresql.org/docs/9.4/static/rules-materializedviews.html]

[https://hashrocket.com/blog/posts/materialized-view-strategies-using-postgresql#example-domain]