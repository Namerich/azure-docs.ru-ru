---
title: Руководство по началу работы с анализом данных с помощью выделенных пулов SQL
description: В этом учебнике вы будете использовать пример данных по такси Нью-Йорка для изучения аналитических возможностей пула SQL.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 11/17/2020
ms.openlocfilehash: ae7b15bf2e2efd72184110a105f68815516569ab
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843916"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Анализ данных с помощью выделенных пулов SQL

Azure Synapse Analytics позволяет анализировать данные с помощью выделенного пула SQL. В этом руководстве показано, как использовать данные о такси Нью-Йорка для изучения возможностей выделенного пула SQL.

## <a name="load-the-nyc-taxi-data-into-sqlpool1"></a>Загрузка данных такси Нью-Йорка в SQLPOOL1

1. В Synapse Studio перейдите в центр **Разработка**, а затем создайте скрипт SQL.
1. В разделе Connect to (Подключиться к) скрипта выберите пул SQLPOOL1 (создан на [шаге 1](https://docs.microsoft.com/azure/synapse-analytics/get-started-create-workspace#create-a-sql-pool) этого учебника).
1. Введите приведенный ниже код.
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Этот скрипт завершит работу приблизительно через 60 секунд. Он загружает 2 млн строк данных такси Нью-Йорка в таблицу с именем **dbo.Trip**.

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Обзор данных о такси Нью-Йорка в выделенном пуле SQL

1. В Synapse Studio перейдите в центр **Данные**.
1. Выберите **SQLPOOL1** > **Таблицы**. Вы увидите, что несколько таблиц загружены.
1. Щелкните правой кнопкой мыши таблицу **dbo.Trip** и выберите команду **Создать скрипт SQL** > **Выбрать первые 100 строк**.
1. Подождите, пока новый скрипт SQL будет создан и запущен.
1. Обратите внимание, что в верхней части скрипта SQL для параметра **Connect to** (Подключиться к) автоматически задано значение пула SQL с именем **SQLPOOL1**.
1. Замените текст скрипта SQL этим кодом и запустите его.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Этот запрос показывает, как общее время поездки и среднее расстояние поездки связаны с количеством пассажиров.
1. В окне результатов скрипта SQL измените представление с **Обзор** на **График**, чтобы увидеть визуализацию результатов в виде графика.



## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Анализ с помощью Spark](get-started-analyze-spark.md)

