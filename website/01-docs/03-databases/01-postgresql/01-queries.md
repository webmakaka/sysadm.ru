---
layout: page
title: Запросы к базе PostgreSQL
description: Запросы к базе PostgreSQL
keywords: Linux, Запросы к базе PostgreSQL
permalink: /adm/databases/postgresql/queries/
---

# Запросы к базе PostgreSQL

<br/>

```sql
-- Создать таблицу из 1 000 000 строк
create table test as
select generate_series as id
	, generate_series::text || (random() * 10)::text as col2
from generate_series(1, 1000000);

```

<br/>

```sql
-- Создать индекс
CREATE INDEX idx_test_01 ON test (col2);
```

<br/>

```sql
-- Посмотреть размер индекса
select indexname, pg_size_pretty(pg_relation_size(indexname::regclass)) as size
from pg_indexes
where tablename = 'test';
```

<br/>

```sql
-- Обновить строки
update test set col2= '101011' || col2 || 'text';
```

<br/>

```sql
-- Перестроить индекс
REINDEX INDEX idx_test_01;
```
