> База данных - это упорядоченный набор структурированной информации или данных, которые обычно хранятся в электронном виде в компьютерной системе

# ACID Properties
## A - atomicity
> Гарантия, что транзакция будет выполнена целиком, или не будет выполнена вообще

Например: 
1. У вас деньги списались
2. Магазину поступили
Либо выполнятся оба действия, либо ничего не выполнится.
## C - consistency
> Гарантия, что после завершения транзации БД должна быть в консистентом состоянии, т.е. операция должна вернуть только итог, никаких промежуточных состояний

Например:
1. Направляется запрос в БД на добавление имени и почты, они должны быть атомарны по свойству выше.
2. В БД должны добавится оба поля и они должны быть как-то связаны (допустим, что они улетают по разным таблицам)
3. Консистентность должна заключаться в том, чтобы в БД не добавилась только почта без имени в связях, т.к. такая запись делает БД неконсистентной.
## I - isolation
> Изоляция гарантирует, что все транзакции друг от друга независимы
## D - Durability
> Гарантия, что если пользователь получил подтверждение от системы, что транзакция выполнена, он может быть уверен, что сделанные им изменения не будут отменены из-за какого-либо сбоя.

# SQL 
> Structured Language Query - это языковая структура запросов, использующаяся в базах данных
# Types
* [Relational](resources/relationaldb.md)
* [NoSQL](resources/nosqldb.md)

# Indexing
# Query Optimization