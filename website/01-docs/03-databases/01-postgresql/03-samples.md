---
layout: page
title: Примеры команд Postgres
description: Примеры команд Postgres
keywords: databases, linux, export, samples
permalink: /adm/databases/postgresql/samples/
---

# Примеры команд Postgres

```
-- 1. написать функцию, считающую куб целого числа на языке SQL, при этом при значении 0 и меньше функция должна вернуть 0
-- https://www.postgresql.org/docs/current/functions-conditional.html
CREATE OR REPLACE FUNCTION kube(int) RETURNS int AS $$
    SELECT CASE WHEN $1>0 THEN $1*$1*$1 ELSE 0 END;
$$ IMMUTABLE
LANGUAGE SQL;

SELECT kube(10);
SELECT kube(-10);
```

<br/>

```
-- 2. написать функцию, считающую куб целого числа на языке pl/pgSQL, при этом при значении 0 и меньше функция должна вернуть 0
CREATE OR REPLACE FUNCTION kube2(int) RETURNS int AS $$
BEGIN
    CASE
        WHEN $1>0
            THEN RETURN $1*$1*$1;
        ELSE
            RETURN 0;
    END CASE;
END;
$$ IMMUTABLE
LANGUAGE PLPGSQL;

SELECT kube2(10);
SELECT kube2(-10);
```

<br/>

```
-- 3. написать процедуру, считающую куб целого числа на языке pl/pgSQL, при этом при значении 0 и меньше процедура должна вернуть 0
CREATE OR REPLACE PROCEDURE kube3(INOUT i int) AS $$
BEGIN
    CASE
        WHEN i>0
            THEN i = i*i*i;
        ELSE
            i = 0;
    END CASE;
END;
$$ LANGUAGE PLPGSQL;

CALL kube3(10);
CALL kube3(-10);
```

<br/>

```
* написать триггер на изменение данных для таблицы, в которой есть два текстовых поля - одно регистрозависимое,
второе поле мы заполняем маленькими буквами для организации регистронезависимого поиска через LIKE
drop TABLE if test_like;
CREATE TABLE test_like (
    txt text,
    txt_lower text
);

-- создаем триггерную функцию
CREATE or replace FUNCTION tr_test_like() RETURNS trigger AS $tr_test_like$
BEGIN
    new.txt_lower := lower(new.txt);
    RETURN new;
END;
$tr_test_like$
LANGUAGE plpgsql;

-- создаем триггер на основе триггерной функции
CREATE TRIGGER tr_test_like BEFORE INSERT OR UPDATE
ON test_like FOR EACH ROW EXECUTE FUNCTION tr_test_like();


INSERT INTO test_like(txt) VALUES ('IvAn');
SELECT * FROM test_like;
SELECT * FROM test_like WHERE txt like '%ivan%';
SELECT * FROM test_like WHERE txt_lower like '%ivan%';
```
