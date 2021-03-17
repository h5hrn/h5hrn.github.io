---
layout: post
title:  "PostgreSQL における UUID の生成"
categories: PostgreSQL
---

### UUID とは

- UUID (Universally Unique Identifier) とは、重複する可能性が限りなく低い方法で生成される128ビットの数値である。
- [RFC 4122](https://tools.ietf.org/html/rfc4122) にて定義されている。

### PostgreSQL で UUID を使う

PostgreSQL のバージョンは13.2である。

```sql
postgres=# SELECT version();
                                                version
-------------------------------------------------------------------------------------------------------
 PostgreSQL 13.2 on x86_64-pc-linux-musl, compiled by gcc (Alpine 10.2.1_pre1) 10.2.1 20201203, 64-bit
```

`pgcrypto` モジュールがなければ追加する。

```sql
postgres=# CREATE EXTENSION if not exists pgcrypto;
CREATE EXTENSION
```

`gen_random_uuid()` で version 4 の UUID を生成する。

```sql
postgres=# SELECT gen_random_uuid();
           gen_random_uuid
--------------------------------------
 663b13a6-3e8d-4b07-9852-c1567a733ed1
```

他バージョンの UUID を使いたい場合は、[uuid-ossp](https://www.postgresql.org/docs/13/uuid-ossp.html) モジュールの関数を使用する。

テーブルのカラムのデフォルト値に設定すれば、UUID が自動で採番されるようになる。

```sql
postgres=# CREATE TABLE test (
postgres(#   id uuid default gen_random_uuid(),
postgres(#   name varchar(40)
postgres(# );
CREATE TABLE
postgres=# INSERT INTO test (name) VALUES ('foo');
INSERT 0 1
postgres=# INSERT INTO test (name) VALUES ('bar');
INSERT 0 1
postgres=# SELECT * FROM test;
                  id                  | name
--------------------------------------+------
 3df28e45-55ff-4ddb-895c-18b355cceee3 | foo
 af87b345-f0e9-4b96-a8b5-397e013e0f27 | bar
(2 rows)
```

`RETURNING` 句を使うと、`INSERT` で生成された id (UUID) を取得できる。 

```sql
postgres=# INSERT INTO test (name) VALUES ('baz') RETURNING id;
                  id
--------------------------------------
 3cf886dd-dab3-4c71-b976-b60c63003601
(1 row)

INSERT 0 1
```

### 参考

- [PostgreSQL: Documentation: 13: F.25. pgcrypto](https://www.postgresql.org/docs/13/pgcrypto.html)
- [PostgreSQL: Documentation: 13: 9.14. UUID Functions](https://www.postgresql.org/docs/13/functions-uuid.html)
- [PostgreSQL: Documentation: 13: 8.12. UUID Type](https://www.postgresql.org/docs/13/datatype-uuid.html)
- [PostgreSQL: Documentation: 13: 6.4. Returning Data from Modified Rows](https://www.postgresql.org/docs/13/dml-returning.html)
