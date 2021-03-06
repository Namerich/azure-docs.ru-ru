---
title: Параметры миграции Cosmos DB
description: В этом документе описываются различные варианты переноса локальных или облачных данных в Azure Cosmos DB
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: 8721c0eb728f568521e86baecb658dc9c869a7f6
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097589"
---
# <a name="options-to-migrate-your-on-premises-or-cloud-data-to-azure-cosmos-db"></a>Параметры для переноса локальных или облачных данных в Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Можно загружать данные из различных источников данных в Azure Cosmos DB. Поскольку Azure Cosmos DB поддерживает несколько интерфейсов API, целевыми объектами могут быть любые существующие API. Ниже приведены некоторые сценарии, в которых данные переносятся в Azure Cosmos DB:

* Перемещение данных из одного контейнера Azure Cosmos в другой контейнер в той же или другой базе данных.
* Перемещение данных между выделенными контейнерами в контейнеры общих баз данных.
* Перемещение данных из учетной записи Azure Cosmos, расположенной в Region1, в другую учетную запись Azure Cosmos в том же или другом регионе.
* Перемещение данных из источника, такого как хранилище BLOB-объектов Azure, файл JSON, база данных Oracle, Couchbase, DynamoDB в Azure Cosmos DB.

Для поддержки путей перехода из различных источников в различные Azure Cosmos DB API существует несколько решений, которые обеспечивают специализированную обработку для каждого пути переноса. В этом документе перечислены доступные решения и описаны их преимущества и ограничения.

## <a name="factors-affecting-the-choice-of-migration-tool"></a>Факторы, влияющие на выбранный инструмент миграции

Выбор средства миграции определяется следующими факторами.

* **Интерактивная и автономная миграция** . Многие средства миграции предоставляют путь для однократной миграции. Это означает, что приложения, обращающиеся к базе данных, могут столкнуться с периодом простоя. Некоторые решения по переносу позволяют выполнять динамическую миграцию, когда между источником и целевым объектом устанавливается конвейер репликации.

* **Источник данных** . существующие данные могут находиться в различных источниках данных, таких как Oracle DB2, Datastax Кассанда, база данных SQL Azure, PostgreSQL и т. д. Данные могут также находиться в существующей учетной записи Azure Cosmos DB и цель миграции — изменить модель данных или выполнить секционирование данных в контейнере с помощью другого ключа секции.

* **Azure Cosmos DB API** : для API SQL в Azure Cosmos DB существует множество средств, разработанных группой Azure Cosmos DB, которые помогают в различных сценариях миграции. Все остальные API-интерфейсы имеют собственный специализированный набор инструментов, разработанных и обслуживаемых сообществом. Поскольку Azure Cosmos DB поддерживает эти API на уровне кабельного протокола, эти средства должны работать как есть во время переноса данных в Azure Cosmos DB. Однако они могут потребовать настраиваемой обработки для регулирования, так как эта концепция характерна для Azure Cosmos DB.

* **Размер данных** . Большинство средств миграции очень хорошо работает для небольших DataSet. Если набор данных превышает несколько сотен гигабайт, выбор средств миграции ограничен. 

* **Ожидаемая длительность миграции** . миграцию можно настроить так, чтобы она настроилась в заданный период времени, который потребляет меньше пропускной способности или может использовать всю пропускную способность, подготовленную в целевом контейнере Azure Cosmos DB, и выполнить миграцию за меньшее время.

## <a name="azure-cosmos-db-sql-api"></a>API SQL для Azure Cosmos DB

|Тип перемещения|Решение|Поддерживаемые источники|Поддерживаемые целевые объекты|Рекомендации|
|---------|---------|---------|---------|---------|
|Вне сети|[Инструмент переноса данных](import-data.md)| &bull;Файлы JSON/CSV<br/>&bull;API SQL для Azure Cosmos DB<br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Хранилище таблиц<br/>&bull;AWS DynamoDB<br/>&bull;Хранилище BLOB-объектов Azure|&bull;API SQL для Azure Cosmos DB<br/>&bull;API таблиц Azure Cosmos DB<br/>&bull;Файлы JSON |&bull; Простота настройки и поддержки нескольких источников. <br/>&bull; Не подходит для больших наборов данных.|
|Вне сети|[Фабрика данных Azure](../data-factory/connector-azure-cosmos-db.md).| &bull;Файлы JSON/CSV<br/>&bull;API SQL для Azure Cosmos DB<br/>&bull;API Azure Cosmos DB для MongoDB<br/>&bull;MongoDB <br/>&bull;SQL Server<br/>&bull;Хранилище таблиц<br/>&bull;Хранилище BLOB-объектов Azure <br/> <br/>Другие поддерживаемые источники см. в статье [фабрика данных Azure](../data-factory/connector-overview.md) .|&bull;API SQL для Azure Cosmos DB<br/>&bull;API Azure Cosmos DB для MongoDB<br/>&bull;Файлы JSON <br/><br/> Другие поддерживаемые целевые объекты см. в статье [фабрика данных Azure](../data-factory/connector-overview.md) . |&bull; Простота настройки и поддержки нескольких источников.<br/>&bull; Использует библиотеку Azure Cosmos DBного выполнителя. <br/>&bull; Подходит для больших наборов данных. <br/>&bull; Отсутствие контрольных точек — это означает, что если проблема возникает во время миграции, необходимо перезапустить весь процесс миграции.<br/>&bull; Отсутствие очереди недоставленных сообщений — это означает, что несколько ошибочных файлов могут прерывать весь процесс миграции.|
|Автономная миграция|[Соединитель Azure Cosmos DB Spark](spark-connector.md)|Azure Cosmos DB API SQL. <br/><br/>Вы можете использовать другие источники с дополнительными соединителями из экосистемы Spark.| Azure Cosmos DB API SQL. <br/><br/>Вы можете использовать другие целевые объекты с дополнительными соединителями из экосистемы Spark.| &bull; Использует библиотеку Azure Cosmos DBного выполнителя. <br/>&bull; Подходит для больших наборов данных. <br/>&bull; Требуется пользовательская настройка Spark. <br/>&bull; Spark учитывает несоответствия схем, и это может быть проблемой во время миграции. |
|Автономная миграция|[Пользовательский инструмент с библиотекой Cosmos DBного выполнителя](migrate-cosmosdb-data.md)| Источник зависит от пользовательского кода. | API SQL для Azure Cosmos DB| &bull; Предоставляет возможности создания контрольных точек, недоставленных сообщений, что повышает устойчивость к миграции. <br/>&bull; Подходит для очень больших наборов данных (10 ТБ +).  <br/>&bull; Требуется пользовательская установка этого инструмента, запускаемого в качестве службы приложений. |
|Миграция по сети|[Функции Cosmos DB и API пр](change-feed-functions.md)| API SQL для Azure Cosmos DB | API SQL для Azure Cosmos DB| &bull; Простота настройки. <br/>&bull; Работает только в том случае, если источником является контейнер Azure Cosmos DB. <br/>&bull; Не подходит для больших наборов данных. <br/>&bull; Не фиксирует операции удаления из исходного контейнера. |
|Миграция по сети|[Настраиваемая служба миграции с использованием пр](https://github.com/Azure-Samples/azure-cosmosdb-live-data-migrator)| API SQL для Azure Cosmos DB | API SQL для Azure Cosmos DB| &bull; Обеспечивает отслеживание хода выполнения. <br/>&bull; Работает только в том случае, если источником является контейнер Azure Cosmos DB. <br/>&bull; Работает также с большими наборами данных.<br/>&bull; Требует, чтобы пользователь настроил службу приложений для размещения обработчика веб-канала изменений. <br/>&bull; Не фиксирует операции удаления из исходного контейнера.|
|Миграция по сети|[стриим](cosmosdb-sql-api-migrate-data-striim.md)| &bull;Oracle; <br/>&bull;Apache Cassandra<br/><br/> Другие поддерживаемые источники см. на [веб-сайте Стриим](https://www.striim.com/sources-and-targets/) . |&bull;API SQL для Azure Cosmos DB <br/>&bull; Azure Cosmos DB API Cassandra<br/><br/> Другие поддерживаемые целевые объекты см. на [веб-сайте Стриим](https://www.striim.com/sources-and-targets/) . | &bull; Работает с большим разнообразием источников, таких как Oracle, DB2 SQL Server.<br/>&bull; Легко создавайте конвейеры ETL и предоставляет панель мониторинга для мониторинга. <br/>&bull; Поддерживает большие наборы данных. <br/>&bull; Поскольку это средство стороннего производителя, оно должно быть приобретено в Marketplace и установлено в среде пользователя.|

## <a name="azure-cosmos-db-mongo-api"></a>Azure Cosmos DB API Mongo

|Тип перемещения|Решение|Поддерживаемые источники|Поддерживаемые целевые объекты|Рекомендации|
|---------|---------|---------|---------|---------|
|Справка в Интернете|[Azure Database Migration Service](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB|API Azure Cosmos DB для MongoDB |&bull; Использует библиотеку Azure Cosmos DBного выполнителя. <br/>&bull; Подходит для больших наборов данных и отвечает за репликацию динамических изменений. <br/>&bull; Работает только с другими источниками MongoDB.|
|Автономная миграция|[Миграция баз данных Azure](../dms/tutorial-mongodb-cosmos-db-online.md)| MongoDB| API Azure Cosmos DB для MongoDB| &bull; Использует библиотеку Azure Cosmos DBного выполнителя. <br/>&bull; Подходит для больших наборов данных и отвечает за репликацию динамических изменений. <br/>&bull; Работает только с другими источниками MongoDB.|
|Вне сети|[Фабрика данных Azure](../data-factory/connector-azure-cosmos-db.md).| &bull;Файлы JSON/CSV<br/>&bull;API SQL для Azure Cosmos DB<br/>&bull;API Azure Cosmos DB для MongoDB <br/>&bull;MongoDB<br/>&bull;SQL Server<br/>&bull;Хранилище таблиц<br/>&bull;Хранилище BLOB-объектов Azure <br/><br/> Другие поддерживаемые источники см. в статье [фабрика данных Azure](../data-factory/connector-overview.md) . | &bull;API SQL для Azure Cosmos DB<br/>&bull;API Azure Cosmos DB для MongoDB <br/>&bull; Файлы JSON <br/><br/> Другие поддерживаемые целевые объекты см. в статье [фабрика данных Azure](../data-factory/connector-overview.md) .| &bull; Простота настройки и поддержки нескольких источников. <br/>&bull; Использует библиотеку Azure Cosmos DBного выполнителя. <br/>&bull; Подходит для больших наборов данных. <br/>&bull; Отсутствие контрольных точек означает, что любые проблемы во время миграции потребуют перезапуска всего процесса миграции.<br/>&bull; Отсутствие очереди недоставленных сообщений означает, что несколько ошибочных файлов могут прерывать весь процесс миграции. <br/>&bull; Требуется пользовательский код для увеличения пропускной способности чтения для определенных источников данных.|
|Вне сети|[Существующие инструменты Mongo (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|MongoDB | API Azure Cosmos DB для MongoDB| &bull; Простота настройки и интеграции. <br/>&bull; Требуется настраиваемая обработка для регулирования.|

## <a name="azure-cosmos-db-cassandra-api"></a>API Cassandra для Azure Cosmos DB

|Тип перемещения|Решение|Поддерживаемые источники|Поддерживаемые целевые объекты|Рекомендации|
|---------|---------|---------|---------|---------|
|Автономная миграция|[cqlsh COPY, команда](cassandra-import-data.md#migrate-data-using-cqlsh-copy-command)|CSV-файлы | API Cassandra для Azure Cosmos DB| &bull; Простота настройки. <br/>&bull; Не подходит для больших наборов данных. <br/>&bull; Работает, только если источником является таблица Cassandra.|
|Автономная миграция|[Копирование таблицы с помощью Spark](cassandra-import-data.md#migrate-data-using-spark) | &bull;Apache Cassandra<br/>&bull;API Cassandra для Azure Cosmos DB| API Cassandra для Azure Cosmos DB | &bull; Может использовать возможности Spark для параллельного преобразования и приема. <br/>&bull; Для управления регулированием требуется конфигурация с настраиваемой политикой повтора.|
|Миграция по сети|[Стриим (из Oracle DB или Apache Cassandra)](cosmosdb-cassandra-api-migrate-data-striim.md)| &bull;Oracle;<br/>&bull;Apache Cassandra<br/><br/> Другие поддерживаемые источники см. на [веб-сайте Стриим](https://www.striim.com/sources-and-targets/) .|&bull;API SQL для Azure Cosmos DB<br/>&bull;API Cassandra для Azure Cosmos DB <br/><br/> Другие поддерживаемые целевые объекты см. на [веб-сайте Стриим](https://www.striim.com/sources-and-targets/) .| &bull; Работает с большим разнообразием источников, таких как Oracle, DB2 SQL Server. <br/>&bull; Легко создавайте конвейеры ETL и предоставляет панель мониторинга для мониторинга. <br/>&bull; Поддерживает большие наборы данных. <br/>&bull; Поскольку это средство стороннего производителя, оно должно быть приобретено в Marketplace и установлено в среде пользователя.|
|Миграция по сети|[Блитзз (из Oracle DB или Apache Cassandra)](oracle-migrate-cosmos-db-blitzz.md)|&bull;Oracle;<br/>&bull;Apache Cassandra<br/><br/>Другие поддерживаемые источники см. на [веб-сайте блитзз](https://www.blitzz.io/) . |API Cassandra Azure Cosmos DB. <br/><br/>Другие поддерживаемые целевые объекты см. на [веб-сайте блитзз](https://www.blitzz.io/) . | &bull; Поддерживает большие наборы данных. <br/>&bull; Поскольку это средство стороннего производителя, оно должно быть приобретено в Marketplace и установлено в среде пользователя.|

## <a name="other-apis"></a>Other APIs

Для API-интерфейсов, отличных от API SQL, Mongo API и API Cassandra, существуют различные средства, поддерживаемые всеми экосистемами API. 

**API таблиц** 

* [Средство миграции данных](table-import.md#data-migration-tool)
* [AzCopy](table-import.md#migrate-data-by-using-azcopy)

**API Gremlin** ;

* [Библиотека массовых исполнителей Graph](bulk-executor-graph-dotnet.md)
* [Gremlin Spark](https://github.com/Azure/azure-cosmosdb-spark/blob/2.4/samples/graphframes/main.scala) 

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения см. в примерах приложений, использующих библиотеку небольшого выполнителя в [.NET](bulk-executor-dot-net.md) и [Java](bulk-executor-java.md). 
* Библиотека небольшого Исполнительного исполнителя интегрирована в соединитель Cosmos DB Spark. Дополнительные сведения см. в статье о [соединителе Azure Cosmos DB Spark](spark-connector.md) .  
* Обратитесь к группе разработчиков Azure Cosmos DB, открыв запрос в службу поддержки в подтипе проблем "Общие рекомендации" и "крупные (ТБ +) миграции" для получения дополнительной справки о крупномасштабных миграциях.
