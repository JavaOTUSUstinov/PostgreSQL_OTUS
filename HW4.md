## HW 14. Логический уровень
### Выполнение по пунктам

1. создайте новый кластер PostgresSQL 14
```bash
SELECT version();
```
PostgreSQL 14.1, compiled by Visual C++ build 1914, 64-bit

2. зайдите в созданный кластер под пользователем postgres
```bash
SELECT current_database();
```
Postgres
```bash
select current_role; 
```
postgres

3. создайте новую базу данных testdb
```bash
CREATE DATABASE testdb;
```
4. зайдите в созданную базу данных под пользователем postgres
```bash
SELECT current_database();
```
Testdb
```bash
select current_role;
```
postgres
5. создайте новую схему testnm
```bash
CREATE SCHEMA testnm;
```
6. создайте новую таблицу t1 с одной колонкой c1 типа integer
```bash
create table t1 (c1 int);
```
7. вставьте строку со значением c1=1
```bash
insert into t1 values (1); --with autocommit
```
8. создайте новую роль readonly
```bash
CREATE role readonly;
```
9. дайте новой роли право на подключение к базе данных testdb
```bash
grant connect on DATABASE testdb TO readonly;
```
10. дайте новой роли право на использование схемы testnm
```bash
grant usage on SCHEMA testnm to readonly;
```
11. дайте новой роли право на select для всех таблиц схемы testnm
```bash
grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
```
12. создайте пользователя testread с паролем testread
CREATE USER testread with password 'testread';
```
13. дайте роль readonly пользователю testread
```bash
grant readonly TO testread;
```
14. зайдите под пользователем testread в базу данных testdb
```bash
SELECT current_database();
```
Testdb
```bash
select current_role;
```
testread
15-21. сделайте select * from t1;
SQL Error [42501]: ОШИБКА: нет доступа к таблице t1
Данную таблицу нашел в схеме public, т.к. таблица t1 была создана из под postgres и создалась в схеме по умолчанию.

22-23. вернитесь в базу данных testdb под пользователем postgres. Удалите таблицу t1
```bash
drop table t1;
```
24. создайте ее заново, но уже с явным указанием имени схемы testnm
```bash
create table testnm.t1 (c1 int);
```
25. вставьте строку со значением c1=1
```bash
insert into testnm.t1 values (1);
```
26-29. зайдите под пользователем testread в базу данных testdb. Сделайте select * from testnm.t1;
select * from t1;

SQL Error [42P01]: ОШИБКА: отношение "t1" не существует

повторно выполняем grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
```bash
select * from testnm.t1;
```
успешно, видимо при создании схемы ей выдаются некие дефолтные гранты

40. если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды
не сам, подсматривал =)
```bash
REVOKE CREATE on SCHEMA public FROM public; 
REVOKE ALL on DATABASE testdb FROM public;
``` 

41. теперь попробуйте выполнить команду 
```bash
create table t3(c1 integer); insert into t2 values (2);
```
SQL Error [42501]: ОШИБКА: нет доступа к схеме public
забрали гранты на создание объектов в public
