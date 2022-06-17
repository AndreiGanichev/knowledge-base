---
date: 2022-06-16
tags:
    - tag
---

Согласо FLP impossibility в асинхронной системе консенсус невозможен за конечное время. Но добавление failure detection(FD) алгоритма позволяет достигать консенсуса даже в асинхронной системе. В этом случае алгоритм консенсуса полагается на FD алгоритм в вопросе определения отказавших нод.

Согласно [1] консенсус будет возможен даже если FD будет делать бесконечное число ошибок. Failure detection алгоритм будет ошибаться(*accuracy* of FD), помечая работающие ноды как отказавшие, алгоритм консенсуса будет перезапускаться, но в итоге(*completeness* of FD) в системе будет корректная информация о состоянии нод и консенсус будет достигнут.

Комбинируя алгоритмы консенсуса и FD в async системе в итоге получаем:
1. *liveness* - конесус не зависнет навечно - обеспечивает FD алгоритмом, который скажет, что нода отказала и консенсус перезапуститься
1. *safety* - обеспечивает алгоритмом консенсуса, это свойство есть у консенсуса вне зависимости от того синхронна система или нет

---

### Источники:
1. Alex Petrov. Database Internals. Failure detection. Summary
2. Alex Petrov. Database Internals. Consensus

### Ссылки:
1. [Chandra, Tushar Deepak, and Sam Toueg. 1996. “Unreliable failure detectors for reliable distributed systems.” Journal of the ACM 43, no. 2 (March): 225-267.](https://doi.org/10.1145/226643.226647)
1. [[Consensus]]
1. [[Failure detection]]
1. [[FLP impossibility]]
1. [[Distributed algorithms]]