libfpta
============================================
Fast Positive Tables, aka "Позитивные Таблицы"
by [Positive Technologies](https://www.ptsecurity.ru).


## Кратко

"Позитивные Таблицы" (_ПТ_) является встраиваемым движком хранения,
который предоставляет интерфейс для управления простыми таблицами и
выполнения примитивных реляционных запросов в локальной совместно
используемой базе данных.

"Позитивные Таблицы" отличаются специфичным набором компромиссов между
производительностью и универсальностью, что дает уникальных набор
возможностей:

1. Одновременный многопоточный доступ к данным из нескольких процессов на
одном сервере Linux.

2. Обслуживание нескольких читателей без блокировок с линейным
масштабированием производительности по ядрам CPU.
  > Для читателей блокировки используются только при подключении и
  > отключении от базы данных. Операции изменения данных никак не блокируют
  > читателей.

3. Строго последовательные изменения без затрат на конкурирующие
блокировок (livelock) и с гарантией от взаимоблокировки (deadlock).
  > В каждый момент времени может быть только один писатель (процесс
  > изменяющий данные).

4. Прямой доступ к данным без накладных расходов.
  > База данных отображается в память. Доступ к данным возможен без
  > лишнего копирования, без выделения памяти, без обращения к сервисам
  > операционной системы.

5. Полная поддержка ACID поверх MVCC. Устойчивость к сбоям и отсутствие
фазы восстановления. Возможность изменения данных только в памяти с
 отложенной асинхронной фиксацией на диске.

"Позитивные Таблицы" опираются на [libfptu](https://github.com/leo-yuriev/libfptu) (aka "Позитивные Кортежи")
для представления данных и на [libmdbx](https://github.com/ReOpen/libmdbx) (aka "eXtended LDMB")
для их хранения.

Однако, "Позитивные Таблицы" не являются серебряной пулей и вероятно не
подойдут, если:

 * Размер одной записи (строки в таблице) всегда или намного больше 4
   килобайт.
 * В запросах требуется обращаться одновременно к нескольким таблицам.
 * Объем данных превышает RAM, либо сценарии использования предполагают
   наличие WAL (Write Ahead Log) или журнала транзакций.
