#ДЗ №1
1. нет, во второй сессии запись не видна, т.к. изменения первой сессии не были применены (commit не выполнен)
2. да, теперь новая запись видна, т.к. в первой сессии выполнен коммит и теперь все прочие сессии после коммита будут видеть данную запись в рамках той же сессии. Т.е. селект получает закоммиченные данные во время выполпнения селекта (особенность read commited)
3. нет, т.к. не выполнен коммит
4. нет, т.к. не смотря на то, что выполнен коммит, транзакция во 2ой сессиий не была завершена. Т.е. селект получает закоммиченные данные на время старта транзакции, а не выполнения селекта (особенность repeatable read)
5. да, т.к. транзакция с селектом во второй сессиий была запущена, после завершения транзакции в первой сессии
