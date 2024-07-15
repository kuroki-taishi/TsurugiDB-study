# DB に触ってみる

- Docker Container は Docker Desktop の Exec から操作する（面倒だから）。

## 3.2 Hello World

## 3.2.1 コンソール起動

```bash
tsurugi@5de7c24ec0a0:/usr/lib/tsurugi-1.0.0-BETA4$ $TSURUGI_HOME/bin/tgsql --connection tcp://localhost:12345
[main] INFO com.tsurugidb.console.core.ScriptRunner - establishing connection: tcp://localhost:12345
[main] INFO com.tsurugidb.console.core.ScriptRunner - start repl
tgsql> \exit
[main] INFO com.tsurugidb.console.core.ScriptRunner - repl execution was successfully completed
tsurugi@5de7c24ec0a0:/usr/lib/tsurugi-1.0.0-BETA4$
```

### 3.2.2 SQL の実行

```sql
tgsql> begin long transaction include ddl;
transaction started. option=[
  type: LTX
  include_ddl: true
]
Time: 4.278 ms
tgsql> create table customer (
     | c_id bigint primary key,
     | c_name varchar(30)  not null,
     | c_age int
     | )
     | ;
execute succeeded
Time: 10.794 ms
tgsql> select * from customer;
[c_id: INT8, c_name: CHARACTER, c_age: INT4]
(0 rows)
Time: 22.793 ms
tgsql> commit;
transaction commit(DEFAULT) finished.
Time: 3.828 ms
tgsql> select * from customer;
start transaction implicitly. option=[
  type: OCC
  label: "tgsql-transaction"
]
Time: 0.621 ms
[c_id: INT8, c_name: CHARACTER, c_age: INT4]
(0 rows)
Time: 1.639 ms
transaction commit(DEFAULT) finished implicitly.
Time: 2.879 ms
tgsql> begin long transaction write preserve 'customer';
transaction started. option=[
  type: LTX
  write_preserve: "customer"
]
Time: 2.977 ms
tgsql> insert into customer values(1, 'Hello', 51);
(1 row inserted)
Time: 11.078 ms
tgsql> insert into customer values(2, 'World', 138);
(1 row inserted)
Time: 1.625 ms
tgsql> insert into customer values(3, 'Tsurugi', 0);
(1 row inserted)
Time: 1.594 ms
tgsql> commit;
transaction commit(DEFAULT) finished.
Time: 18.757 ms
tgsql> select * from customer;
start transaction implicitly. option=[
  type: OCC
  label: "tgsql-transaction"
]
Time: 0.572 ms
[c_id: INT8, c_name: CHARACTER, c_age: INT4]
[1, Hello, 51]
[2, World, 138]
[3, Tsurugi, 0]
(3 rows)
Time: 4.251 ms
transaction commit(DEFAULT) finished implicitly.
Time: 3.076 ms
tgsql> \exit
[main] INFO com.tsurugidb.console.core.ScriptRunner - repl execution was successfully completed
tsurugi@5de7c24ec0a0:/usr/lib/tsurugi-1.0.0-BETA4$ 
```

