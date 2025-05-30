---
date: 2025-02-24
---
# MySql configuration

## Max connections

> Rule of thumb: Max number of connections = 4 times available CPU cores [1]

MySql хорошо обрабатывает нагрузку с short-lived connections:

> MySQL is very good at handling many clients connecting and disconnecting to the database at a high frequency [1]

---

## Источники

1. [MySQL Connection Handling and Scaling](https://dev.mysql.com/blog-archive/mysql-connection-handling-and-scaling/)

## Ссылки

1. link
