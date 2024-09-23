---
date: 2024-07-06
---
# Troubleshooting

# Ресурсы

1. CPU, причем обращаем внимание на что тратится: user/system/iowait. Если system, то это может быть связано с частым переключением контекста, надо посмотреть на кол-во потоков.
1. Memory
1. IO(диск)

# Потоки

[MySQL Threads Running](https://hackmysql.com/mysql-threads-running-how-hard-is-mysql-working/)

# Настройки

1. `innodb_buffer_pool_size` - размер буфера, должен составлять 60-80% от доступной памяти. По умолчаниб 128 Мб.

# Handlers

1. `read_rnd_next` - это счетчик выполнения full scan

# Блокировки

---

## Источники

1. link

## Ссылки

1. [Troubleshooting Common MySQL Performance Issues](https://www.percona.com/blog/troubleshooting-common-mysql-performance-issues/)
1. [MySQL Performance Tuning 101: Key Tips to Improve MySQL Database Performance](https://www.percona.com/blog/mysql-101-parameters-to-tune-for-mysql-performance/)
1. [Reducing High CPU on MySQL: a Case Study](https://www.percona.com/blog/reducing-high-cpu-on-mysql-a-case-study/)