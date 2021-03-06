---
title: 'Преимущества: Миграция локальных Apache Hadoop в Azure HDInsight'
description: Ознакомьтесь с мотивирующими факторами для миграции локальных кластеров Hadoop в Azure HDInsight и получаемыми в результате преимуществами.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 11/15/2019
ms.openlocfilehash: 595bf6f921265e9e8dbc0e0e065fe835efea14bc
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331657"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>Миграция локальных кластеров Apache Hadoop в Azure HDInsight — мотивация и преимущества

Это первая статья в цикле, посвященном рекомендациям по перемещению локальных развертываний экосистемы Apache Hadoop в Azure HDInsight. Этот цикл статей предназначен для людей, которые отвечают за проектирование, развертывание и миграцию решений Apache Hadoop в Azure HDInsight. Она особенно полезна для архитекторов облака, администраторов Hadoop и инженеров DevOps. Разработчикам программного обеспечения, инженерам и специалистам по обработке и анализу данных также пригодятся знания того, как различные типы кластеров работают в облаке.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Миграция в Azure HDInsight

Azure HDInsight является облачным дистрибутивом компонентов Hadoop. Azure HDInsight обеспечивает простую, быструю и экономичную обработку больших объемов данных. HDInsight включает в себя наиболее популярные платформы с открытым исходным кодом, такие как:

- Apache Hadoop
- Apache Spark
- Apache Hive с LLAP
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="azure-hdinsight-advantages-over-on-premises-hadoop"></a>Преимущества Azure HDInsight по сравнению с локальной средой Hadoop

- **Низкая стоимость**. Затраты можно уменьшить за счет [создания кластеров по требованию](../hdinsight-hadoop-create-linux-clusters-adf.md) и платы только за используемые ресурсы. Несвязанные ресурсы вычисления и хранения обеспечивают гибкость благодаря сохранению объема данных независимо от размера кластера.

- **Автоматическое создание кластера**. Автоматическое создание кластера требует минимальной настройки. Автоматизация может использоваться для кластеров по требованию.

- **Управляемое оборудование и конфигурация**. Используя кластер HDInsight, вы больше не будете беспокоиться о физическом оборудовании или инфраструктуре. Просто укажите конфигурацию кластера, и Azure настроит его.

- **Простота** масштабирования — HDInsight позволяет [масштабировать](../hdinsight-administer-use-portal-linux.md) рабочие нагрузки. Azure выполняет перераспределение данных и перебалансировку рабочей нагрузки без прерывания заданий обработки данных.

- **Глобальная доступность** . HDInsight доступна в большем количестве [регионов](https://azure.microsoft.com/regions/services/) , чем любое другое предложение аналитики больших данных. Служба Azure HDInsight также доступна в Azure для государственных организаций, Китая и Германии, что позволяет обеспечить соответствие требованиям организации в основных независимых регионах.

- **Безопасность и соответствие требованиям** — HDInsight позволяет защищать ресурсы данных предприятия с помощью [виртуальной сети Azure](../hdinsight-plan-virtual-network-deployment.md), [шифрования](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md)и интеграции с [Azure Active Directory](../domain-joined/hdinsight-security-overview.md). HDInsight также соответствует наиболее распространенным отраслевым и государственным [стандартам](https://azure.microsoft.com/overview/trusted-cloud).

- **Упрощенное управление версиями** . Azure HDInsight управляет версиями компонентов экономичной системы Hadoop и поддерживает их в актуальном состоянии. Обновления программного обеспечения обычно представляют собой сложный процесс для локальных развертываний.

- **Кластеры меньшего размера, оптимизированные для конкретных рабочих нагрузок с меньшим числом зависимостей между компонентами** . Типичная локальная установка Hadoop использует один кластер, который обслуживает множество целей. С помощью Azure HDInsight можно создавать кластеры, зависящие от рабочей нагрузки. Создание кластеров для конкретных рабочих нагрузок устраняет возрастающую сложность обслуживания одного кластера.

- **Производительность**. Вы можете использовать различные инструменты для Hadoop и Spark в предпочитаемой среде разработки.

- **Расширяемость с помощью пользовательских средств или сторонних приложений** . кластеры HDInsight можно расширить с помощью установленных компонентов. Кроме того, их можно интегрировать с другими решениями для обработки больших данных, используя развертывания [одним щелчком](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) из места рынка Azure.

- **Простота управления, администрирования и мониторинга** . Azure HDInsight интегрируется с [журналами Azure Monitor](../hdinsight-hadoop-oms-log-analytics-tutorial.md) для предоставления единого интерфейса, с помощью которого можно отслеживать все кластеры.

- **Интеграция с другими службами Azure**. HDInsight можно легко интегрировать с другими популярными службами Azure, такими как:

    - Фабрика данных Azure (ADF)
    - хранилище BLOB-объектов Azure
    - Azure Data Lake Storage 2-го поколения
    - Azure Cosmos DB
    - База данных SQL Azure
    - Службы Azure Analysis Services

- **Процессы и компоненты самовосстановления**. HDInsight постоянно проверяет инфраструктуру и компоненты с открытым исходным кодом, используя собственную инфраструктуру мониторинга. HDInsight также автоматически восстанавливает критические сбои, такие как недоступность узлов и компонентов с открытым исходным кодом. В случае сбоя любого компонента OSS в Ambari активируются оповещения.

Дополнительные сведения см. в статье [Что такое Azure HDInsight и стек технологий Apache Hadoop](../hadoop/apache-hadoop-introduction.md).

## <a name="migration-planning-process"></a>Процесс планирования миграции

Для планирования переноса локальных кластеров Hadoop в Azure HDInsight нужно:

1. Разобраться с текущим локальным развертыванием и его топологиями.
2. Разобраться с масштабом, сроками текущего проекта и знаниями команды.
3. Разобраться с требованиями Azure.
4. Составить подробный план, следуя рекомендациям.

## <a name="gathering-details-to-prepare-for-a-migration"></a>Сбор сведений для подготовки к миграции

Этот раздел содержит шаблонные вопросники, помогающие собрать такую важную информацию:

- информацию о локальном развертывании;
- сведения о проекте;
- Требования Azure

### <a name="on-premises-deployment-questionnaire"></a>Анкета локального развертывания

| **Вопрос** | **Пример** | **Ответ** |
|---|---|---|
|**Раздел**: **среда**|||
|Версия распространения кластера|HDP 2.6.5, CDH 5.7|
|Компоненты экосистемы больших данных|HDFS, Yarn, Hive, LLAP, Impala, Kudu, HBase, Spark, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Falcon, Zeppelin, R|
|Типы кластера|Hadoop, Spark, Confluent Kafka, Storm, Solr|
|Число кластеров|4|
|Число главных узлов|2|
|Число рабочих узлов|100|
|Количество граничных узлов| 5|
|Общее используемое дисковое пространство|100 ТБ|
|Конфигурация главного узла|M/Y, ЦП, диск и т. д.|
|Конфигурации узлов данных|M/Y, ЦП, диск и т. д.|
|Конфигурации граничных узлов|M/Y, ЦП, диск и т. д.|
|Используется ли шифрование HDFS?|Да|
|Высокий уровень доступности|HDFS HA, Metastore HA|
|Аварийное восстановление и резервное копирование|Нужно ли резервное копирование кластера?|  
|Системы, зависящие от кластера|SQL Server, Teradata, Power BI, MongoDB|
|Интеграция сторонних продуктов|Tableau, GridGain, Qubole, Informatica, Splunk|
|**Раздел**: **безопасность**|||
|Безопасность периметра|Брандмауэры|
|Аутентификация и авторизация в кластере|Active Directory, Ambari, Cloudera Manager, без аутентификации|
|Управление доступом к HDFS|  Ручной режим, пользователи SSH|
|Аутентификация и авторизация в Hive|Sentry, LDAP, AD с Kerberos, Ranger|
|Аудит|Ambari, Cloudera Navigator, Ranger|
|Наблюдение|Graphite, collectd, statsd, Telegraf, InfluxDB|
|Оповещение|Kapacitor, Prometheus, Datadog|
|Срок хранения данных| 3 года, 5 лет|
|Администраторы кластера|Единый администратор, несколько администраторов|

### <a name="project-details-questionnaire"></a>Вопросник по проекту

|**Вопрос**|**Пример**|**Ответ**|
|---|---|---|
|**Раздел**: **рабочие нагрузки и частота**|||
|Задания MapReduce|10 заданий — дважды в день||
|Задания Hive|100 заданий — каждый час||
|Пакетные задания Spark|50 заданий — каждые 15 минут||
|Задания потоковой передачи Spark|5 заданий — каждые 3 минуты||
|Задания структурированной потоковой передачи|5 заданий — каждую минуту||
|Задания обучения модели машинного обучения|2 задания — один раз в неделю||
|Языки программирования|Python, Scala, Java||
|Написание сценариев|Shell, Python||
|**Раздел**: **данные**|||
|Источники данных|Неструктурированные файлы, JSON, Kafka, RDBMS||
|Оркестрация данных|Рабочие процессы Oozie, Airflow||
|Поиск в памяти|Apache Ignite, Redis||
|Целевое расположение данных|HDFS, RDBMS, Kafka, MPP ||
|**Раздел**: **метаданные**|||
|Тип базы данных Hive|Mysql, Postgres||
|Число метахранилища Hive|2||
|Число таблиц Hive|100||
|Число политик Ranger|20||
|Число рабочих процессов Oozie|100||
|**Раздел**: **масштабирование**|||
|Объем данных, включая репликацию|100 ТБ||
|Ежедневный объем приема данных|50 ГБ||
|Темп роста объема данных|10 % в год||
|Темп увеличения узлов кластера|5 % в год
|**Раздел**: **использование кластера**|||
|Средняя загрузка ЦП (%)|60 %||
|Среднее использование памяти (%)|75 %||
|Используемое дисковое пространство|75 %||
|Среднее использование сети (%)|25 %
|**Раздел**: **персонал**|||
|Число администраторов|2||
|Число разработчиков|10||
|Число конечных пользователей|100||
|Навыки|Hadoop, Spark||
|Количество доступных ресурсов для миграции|2||
|**Раздел**: **ограничения**|||
|Текущие ограничения|Высокая задержка||
|Текущие задачи|Проблема параллелизма||

### <a name="azure-requirements-questionnaire"></a>Вопросник по требованиям Azure

|**Вопрос**|**Пример**|**Ответ**|
|---|---|---|
|**Раздел**: **инфраструктура** |||
| Предпочтительный регион|Восточная часть США||
|Есть ли предпочитаемая виртуальная сеть?|Да||
|Требуется ли аварийное восстановление и высокая доступность?|Да||
|Нужна ли интеграция с другими облачными службами?|ADF, CosmosDB||
|**Раздел**: **перемещение данных**  |||
|Предпочтения к начальной загрузке|DistCp, Data Box, ADF, WANDisco||
|Передача разностных данных|DistCp, AzCopy||
|Текущая добавочная передача данных|DistCp, Sqoop||
|**Раздел**: **мониторинг и оповещения** |||
|Использование возможностей мониторинга и оповещений Azure или интеграция сторонних решений для мониторинга|Использование возможностей мониторинга и оповещений Azure||
|**Раздел**: **настройки безопасности** |||
|Нужны ли закрытый и защищенный конвейеры данных?|Да||
|Используется ли присоединенный к домену кластер (ESP)?|     Да||
|Нужна ли синхронизация локальной службы AD с облаком?|     Да||
|Число пользователей AD для синхронизации?|          100||
|Допускается ли синхронизация паролей с облаком?|    Да||
|Разрешены ли пользователи "только в облаке"?|                 Да||
|Требуется ли MFA?|                       Нет|| 
|Есть ли требования к авторизации данных?|  Да||
|Управление доступом на основе ролей|        Да||
|Требуется ли аудит?|                  Да||
|Нужно ли шифрование неактивных данных?|          Да||
|Нужно ли шифрование данных при передаче?|       Да||
|**Раздел**: **предпочтения касательно перепроектирования** |||
|Отдельный кластер или определенные типы кластеров|Определенные типы кластеров||
|Требуется ли удаленное или совместно размещенное хранилище?|Удаленное хранилище||
|Размер кластера меньший, так как данные хранятся удаленно?|Меньший размер кластера||
|Требуется ли использование нескольких небольших кластеров вместо одного большого?|Использование нескольких небольших кластеров||
|Требуется ли использование удаленного хранилища метаданных?|Да||
|Нужно ли совместное использование хранилищ метаданных между разными кластерами?|Да||
|Нужно ли деконструировать рабочие нагрузки?|Заменить задания Hive заданиями Spark||
|Нужно ли использовать ADF для оркестрации данных?|Нет||

## <a name="next-steps"></a>Дальнейшие шаги

Прочитайте следующую статью в этом цикле:

- [Migrate on-premises Apache Hadoop clusters to Azure HDInsight - architecture best practices](apache-hadoop-on-premises-migration-best-practices-architecture.md) (Миграция локальных кластеров Apache Hadoop в Azure HDInsight. Рекомендации по архитектуре)
