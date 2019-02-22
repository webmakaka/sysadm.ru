---
layout: page
title: Проверка возможности подключения к базе postgresql
permalink: /linux/dev/go/postgresql/
---

# Проверка возможности подключения к базе postgresql

Подключаюсь к бесплатной базе облачного провайдера heroku.

<br/>

    $ go get github.com/lib/pq

<br/>

    $ vi main.go

<br/>

```go
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/lib/pq"
)

const (
	host     = "ec2-23-23-184-76.compute-1.amazonaws.com"
	port     = 5432
	user     = "hrgcmhzjkgllyf"
	password = "f867d132e78e27e50a27d0b7522dbf3f44dc835c903eb3040d74ecd5daf5c633"
	dbname   = "d61hvpjfrp6em7"
	sslmode  = "require"
)

func main() {
	psqlInfo := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=%s", host, port, user, password, dbname, sslmode)

	db, err := sql.Open("postgres", psqlInfo)
	if err != nil {
		panic(err)
	}

	err = db.Ping()
	if err != nil {
		panic(err)
	}

	fmt.Println("Successfully connectd!")
	db.Close()

}
```

Следует обратить внимание, требует ли база подключения по sslmode. Heroku требует.

<br/>

    $ go run main.go
    Successfully connectd!
