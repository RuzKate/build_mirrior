# Построение зеркал
## Описание
Данный проект содержит реализацию процесса для построения зеркал в банковском хранилище с помощью Spark. Зеркало - это таблица в сыром или детальном слое, содержащая последнее (актуальное) состояние каждой сущности  (записи/строки). Процесс объединяет данные из дельт (изменений) в одну итоговую таблицу, сохраняя только последнюю версию каждого объекта.
### Функциональность
- Принимает в качестве входных данных:
    - Путь к директории, содержащей дельты
    - Наименование таблицы
    - Список полей, составляющих уникальный ключ
- Процесс автоматически обрабатывает дельты, расположенные в поддиректориях с возрастающим номером (например, 1000, 1001...)
- Сохраняет итоговый результат в CSV-файл в директории ```mirr_md_account_d```
- Ведет логирование обработанных дельт, чтобы предотвратить повторную обработку и оптимизировать расход ресурсов
- Логи храниться в виде отдельных CSV-файлов
- В логах будут отражены: время начала и завершения загрузки каждой дельты, наименование обновляемой таблицы, идентификатор дельты

#### Дополнительные сведения 
 Для демонстрации работоспособности процесса было обработано 4 дельты с изменениями некоторых параметров в справочнике счетов. Для проверки механизма логирования, предотвращающего повторную загрузку дельт, была создана дополнительная дельта. Результаты обработки (включая обработку уже загруженной дельты) можно посмотреть в соответствующих директориях: ```data_deltas```, ```logs```, ```mirr_md_account_d``` и ```spark-warehouse```.

## Структура проекта
```data_deltas/``` - папка с директориями, хранящими дельты данных (CSV-файлы)

```logs/``` - папка с файлами логов обработки дельт

```mirr_md_account_d/``` - папка, хранящая итоговый CSV-файл с данными зеркала

```spark-warehouse/md_account_d/``` - папка, хранящая данные зеркала, созданные как Delta Lake таблица

```build_mirrior.ipynb``` - Jupyter Notebook, содержащий код обработки дельт и построения зеркала

## Связанные проекты
[ETL-процесс](https://github.com/RuzKate/etl_process_de.git) - данный проект представляет собой ETL-процесс, написанный на Python и предназначенный для загрузки данных из CSV-файлов в таблицы базы данных PostgreSQL с использованием логирования.

[Data Mart](https://github.com/RuzKate/data_mart_de.git) - этот проект реализует расчет витрин данных в слое «DM» с логированием (витрина оборотов, витрина 101-й отчётной формы).

[Система управления данными](https://github.com/RuzKate/data_management_system.git) - данный проект реализует систему управления данными, предоставляя функциональность для экспорта данных из таблицы в CSV-файл, последующей модификации этих данных и загрузки в новую таблицу в базе данных.
