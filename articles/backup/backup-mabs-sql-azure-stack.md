---
title: Резервное копирование рабочих нагрузок SQL Server в Azure Stack
description: Из этой статьи вы узнаете, как настроить Microsoft Azure Backup Server (MABS) для защиты баз данных SQL Server на Azure Stack.
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 80de7913b010fca69c3703e423109f2ede653590
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91332820"
---
# <a name="back-up-sql-server-on-azure-stack"></a>Резервное копирование SQL Server в Azure Stack

Эта статья может помочь при настройке Microsoft Azure Backup Server (MABS) для защиты баз данных SQL Server в Azure Stack.

Процесс управления резервным копированием и восстановлением базы данных SQL Server в Azure состоит из трех этапов.

1. Создание политики резервного копирования для защиты баз данных SQL Server
2. Создание резервных копий по запросу
3. Восстановление базы данных с дисков и из Azure

## <a name="prerequisites-and-limitations"></a>Предварительные условия и ограничения

* Если у вас есть база данных, файлы которой расположены на удаленном файловом ресурсе, то при включении защиты произойдет сбой с кодом ошибки 104. MABS не поддерживает защиту данных SQL Server на удаленном файловом ресурсе.
* MABS не может защищать базы данных, хранящиеся на удаленных общих ресурсах SMB.
* Убедитесь, что для [реплик группы обеспечения доступности установлен режим "только для чтения"](/sql/database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server).
* Необходимо явно добавить системную учетную запись **NTAuthority\System** в группу Sysadmin на SQL Server.
* При восстановлении частично автономной базы данных в альтернативное расположение убедитесь в том, что в целевом экземпляре SQL активирован параметр [Автономные базы данных](/sql/relational-databases/databases/migrate-to-a-partially-contained-database#enable).
* При восстановлении базы данных файлового потока в альтернативное расположение убедитесь в том, что в целевом экземпляре SQL активирован параметр [База данных файлового потока](/sql/relational-databases/blob/enable-and-configure-filestream).
* Защита для SQL Server AlwaysOn.
  * MABS обнаруживает группы доступности при выполнении запроса при создании группы защиты.
  * MABS обнаруживает отработку отказа и возобновляет защиту базы данных.
  * MABS поддерживает конфигурации кластеров с несколькими сайтами для экземпляра SQL Server.
* При защите баз данных, использующих функцию AlwaysOn, MABS имеет следующие ограничения.
  * MABS будет учитывать политику резервного копирования для групп доступности, которые задаются в SQL Server на основе настроек резервного копирования следующим образом.
    * Предпочитать вторичную: резервное копирование должно выполняться на вторичную реплику, если первичная реплика не является единственной репликой, подключенной к сети. Если доступно несколько вторичных реплик, для резервного копирования будет выбран узел с самым высоким приоритетом резервного копирования. Если доступна только первичная реплика, то резервная копия должна выполняться на первичной реплике.
    * Только вторичная: резервное копирование не должно выполняться на первичную реплику. Если доступна только первичная реплика, то архивирование не будет выполнено.
    * Первичная: резервное копирование всегда выполняется на первичную реплику.
    * Любая реплика: резервное копирование выполняется на любую доступную реплику в группе обеспечения доступности. Узел, с которого будет выполняться резервное копирование, будет определяться по приоритету резервного копирования всех узлов.
  * Следует отметить следующее.
    * Резервные копии могут происходить из любой доступной для чтения реплики, которая является первичной, синхронной вторичной базой данных-получателем.
    * Если какая-либо реплика исключена из резервной копии, например " **Исключить реплику** " включена или помечена как недоступная для чтения, то эта реплика не будет выбрана для резервного копирования при любом из параметров.
    * Если доступно несколько реплик и они доступны для чтения, для резервного копирования будет выбран узел с самым высоким приоритетом резервного копирования.
    * Если резервное копирование на выбранном узле завершается сбоем, операция резервного копирования завершается сбоем.
    * Восстановление в исходное расположение не поддерживается.
* Проблемы при резервном копировании SQL Server 2014 или выше
  * В SQL Server 2014 добавлена новая функция для создания [базы данных для локального экземпляра SQL Server в хранилище BLOB-объектов Azure](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure). MABS нельзя использовать для защиты этой конфигурации.
  * Существуют некоторые известные проблемы с предпочтениями резервного копирования "предпочитать вторичные" для параметра SQL AlwaysOn. MABS всегда выполняет резервное копирование из базы данных-получателя. Если не удается найти базу данных-получатель, произойдет сбой резервного копирования.

## <a name="before-you-start"></a>Перед началом работы

[Установка и подготовка Azure Backup Server](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Создание политики резервного копирования для защиты баз данных SQL Server в Azure

1. В пользовательском интерфейсе Azure Backup Server выберите рабочую область **Защита** .

2. На ленте инструментов выберите **создать** , чтобы создать новую группу защиты.

    ![Создание группы защиты](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup Server запускает мастер группы защиты, который позволяет создать **Группу защиты**. Выберите **Далее**.

3. На экране **Выбор типа группы защиты** выберите **Серверы**.

    ![Выбор типа для группы защиты — «Серверы»](./media/backup-azure-backup-sql/pg-servers.png)

4. На экране **Выбор элементов группы** в списке "Доступные элементы" показаны различные источники данных. Выберите **+** , чтобы развернуть папку и отобразить вложенные папки. Установите флажок, чтобы выбрать элемент.

    ![Выбор базы данных SQL](./media/backup-azure-backup-sql/pg-databases.png)

    Все выбранные элементы отображаются в списке "Выбранные элементы". Выбрав серверы или базы данных, которые необходимо защитить, нажмите кнопку **Далее**.

5. На экране **Выбор метода защиты данных** укажите имя группы защиты и установите флажок **Мне нужна оперативная защита**.

    ![Метод защиты данных — краткосрочная на диске и оперативная в Azure](./media/backup-azure-backup-sql/pg-name.png)

6. На экране **укажите Short-Term цели** включите необходимые входные данные для создания точек резервного копирования на диск и нажмите кнопку **Далее**.

    В этом примере **Диапазон хранения** составляет **5 дней**, **Частота синхронизации** составляет один раз каждые **15 минут**, что является частотой резервного копирования. **Быстрая полная архивация** задано значение **20:00**.

    ![Краткосрочные цели](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > В приведенном примере в 20:00 ежедневно создаются точки резервного копирования путем передачи измененных данных из точки резервного копирования в 20:00 за предыдущий день. Этот процесс называется **быстрой полной архивацией**. Журналы транзакций синхронизируются каждые 15 минут. Если необходимо восстановить базу данных в 21:00, точка восстановления создается из журналов для последней точки быстрого полного резервного копирования (в этом случае в 20:00).
   >
   >

7. На экране **Проверка выделения места на диске** выполните проверку общего доступного объема памяти и сведения о потенциальном дисковом пространстве. Выберите **Далее**.

8. В меню **Выбор метода создания реплики** выберите способ создания первой точки восстановления. Начальную резервную копию можно передать вручную (не по сети), чтобы не перегружать полосу пропускания, либо по сети. При выборе ожидания передачи первой резервной копии можно указать время для начальной передачи. Выберите **Далее**.

    ![Метод начальной репликации](./media/backup-azure-backup-sql/pg-manual.png)

    Начальная резервная копия требует передачи всего источника данных (SQL Server базы данных) с рабочего сервера (SQL Server компьютера) в Azure Backup Server. Объем этих данных может быть велик, и их передача по сети может превысить пропускную способность. По этой причине вы можете выбрать передачу начальной резервной копии **вручную** (с помощью съемного носителя), чтобы избежать перегрузки пропускной способности сети, или **автоматически по сети** (в указанное время).

    После создания начальной резервной копии на ее основе будет выполняться добавочная архивация. Обычно добавочные резервные копии имеют небольшой размер и могут быть легко переданы по сети.

9. Выберите, когда необходимо выполнить проверку согласованности, и нажмите кнопку **Далее**.

    ![Проверка согласованности](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup Server проверяет целостность точки резервного копирования. Azure Backup Server вычисляет контрольную сумму файла резервной копии на рабочем сервере (SQL Server компьютере в этом сценарии) и резервные данные для этого файла. При возникновении конфликта предполагается, что резервный файл на Azure Backup Server поврежден. Azure Backup Server исправляет резервные копии данных, отправляя блоки, соответствующие расхождению в контрольной сумме. Поскольку проверки целостности являются ресурсоемкими, можно запланировать проверку целостности или выполнять ее автоматически.

10. Чтобы указать оперативную защиту источников данных, выберите базы данных для защиты в Azure и нажмите кнопку **Далее**.

    ![Выбор источников данных](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Выбирайте расписание резервного копирования и политики хранения в соответствии с политиками своей организации.

    ![Планирование и хранение](./media/backup-azure-backup-sql/pg-schedule.png)

    В этом примере резервные копии создаются дважды в день — в 12:00 и в 20:00 (в нижней части экрана).

    > [!NOTE]
    > Чтобы обеспечить быстрое восстановление, рекомендуется создать на диске несколько краткосрочных точек восстановления. Эти точки восстановления используются для оперативного восстановления. Azure — это удобное внешнее расположение с более высоким уровнем обслуживания и гарантированной доступностью.
    >
    >

    **Рекомендация**. Если запуск резервного копирования в Azure планируется после завершения резервного копирования локального диска, последние резервные копии диска всегда копируются в Azure.

12. Выберите расписание для политики хранения. Дополнительные сведения о том, как работает политика хранения, см. в статье [Использование службы архивации Azure для замены ленточной инфраструктуры](backup-azure-backup-cloud-as-tape.md).

    ![Политика хранения](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    В этом примере:

    * Резервные копии создаются дважды в день, в 12:00 и в 20:00 (в нижней части экрана), и хранятся в течение 180 дней.
    * Резервная копия, создаваемая в субботу в 12:00, хранится 104 недели.
    * Резервная копия, создаваемая в последнюю субботу в 12:00, хранится 60 месяцев.
    * Резервная копия, создаваемая в последнюю субботу марта в 12:00, хранится 10 лет.
13. Нажмите кнопку **Далее** и выберите подходящий вариант передачи начальной резервной копии в Azure. Вы можете выбрать параметр **Автоматически по сети**.

14. После просмотра сведений о политике на экране **Сводка** выберите **создать группу** , чтобы завершить рабочий процесс. Вы можете выбрать пункт **Закрыть** и отслеживать ход выполнения задания в рабочей области мониторинг.

    ![Создание группы защиты: ход выполнения](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Архивация базы данных SQL Server по запросу

С помощью описанных выше шагов мы создали политику резервного копирования, однако «точка восстановления» создается только при первом резервном копировании. Можно не ждать создания точки восстановления с помощью планировщика, а создать ее вручную, выполнив описанные ниже действия.

1. Прежде чем создавать точку восстановления, подождите, пока в группе защиты для базы данных отобразится состояние **ОК** .

    ![Члены группы защиты](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Щелкните базу данных правой кнопкой мыши и выберите **Создать точку восстановления**.

    ![Создание точки оперативного восстановления](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. В раскрывающемся меню выберите **оперативная защита** и нажмите **кнопку ОК** , чтобы начать создание точки восстановления в Azure.

    ![Создать точку восстановления](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Просмотреть ход выполнения задания в рабочей области **Мониторинг**.

    ![Консоль наблюдения](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Восстановление базы данных SQL Server из Azure

Чтобы восстановить защищенный объект (базу данных SQL Server) из Azure, необходимо выполнить следующие шаги.

1. Откройте консоль управления Azure Backup Server. Перейдите в рабочую область **Восстановление**, где вы увидите защищенные серверы. Найдите нужную базу данных (в данном случае ReportServer$MSDPM2012). Выберите **Восстановление из** времени, указанного в качестве точки в **сети** .

    ![Выбор точки восстановления](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Щелкните правой кнопкой мыши имя базы данных и выберите команду **восстановить**.

    ![Восстановление из Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS отображает сведения о точке восстановления. Выберите **Далее**. Чтобы перезаписать базу данных, выберите тип восстановления **Восстановить в исходном экземпляре SQL Server**. Выберите **Далее**.

    ![Восстановление в исходном расположении](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    В этом примере MABS восстанавливает базу данных в другом экземпляре SQL Server или в отдельной сетевой папке.

4. В окне **Указание параметров восстановления** можно выбрать параметр "Регулирование использования полосы пропускания сети", чтобы регулировать полосу пропускания, используемую при восстановлении. Выберите **Далее**.

5. В окне **Сводка** вы увидите все конфигурации восстановления, доступные на данный момент. Выберите **восстановить**.

    В окне отобразится состояние восстановления, показывающее, что выполняется восстановление базы данных. Можно нажать кнопку **Закрыть** , чтобы закрыть мастер и просмотреть ход выполнения в рабочей области **мониторинг** .

    ![Запуск процесса восстановления](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    По завершении процесса восстановленная база данных будет согласована с приложением.

## <a name="next-steps"></a>Дальнейшие шаги

См. в статье [Файлы резервной копии и приложение](backup-mabs-files-applications-azure-stack.md).
См. в статье [SharePoint резервного копирования на Azure Stack](backup-mabs-sharepoint-azure-stack.md).
