---
title: Перемещение пространства имен концентраторов событий Azure в другой регион | Документация Майкрософт
description: В этой статье показано, как переместить пространство имен концентраторов событий Azure из текущего региона в другой.
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: b177c3916919e3d97325f9d8c6b6027c00cb476f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96019928"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Перемещение пространства имен концентраторов событий Azure в другой регион
В этой статье показано, как экспортировать шаблон Azure Resource Manager для существующего пространства имен концентраторов событий, а затем использовать этот шаблон для создания пространства имен с теми же параметрами конфигурации в другом регионе. Однако этот процесс не перемещает события, которые еще не обработаны. Необходимо обработать события из исходного пространства имен, прежде чем удалять их.
 
При наличии других ресурсов в группе ресурсов Azure, содержащей пространство имен концентраторов событий, может потребоваться экспортировать шаблон на уровне группы ресурсов, чтобы все связанные ресурсы можно было переместить в новый регион за один шаг. Действия, описанные в этой статье, показывают, как экспортировать **пространство имен** в шаблон. Действия по экспорту **группы ресурсов** в шаблон похожи. 

## <a name="prerequisites"></a>Предварительные требования

- Убедитесь, что службы и функции, используемые вашей учетной записью, поддерживаются в целевом регионе.
- Если вы включили **функцию записи** для концентраторов событий в пространстве имен, переместите [хранилище Azure или Azure Data Lake Store gen 2](../storage/common/storage-account-move.md) или [Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-migration-cross-region.md) , прежде чем перемещать пространство имен концентраторов событий. Вы также можете переместить группу ресурсов, содержащую пространства имен хранилища и концентраторов событий, в другой регион, выполнив действия, аналогичные описанным в этой статье. 
- Если пространство имен концентраторов событий находится в **кластере концентраторов событий**, перед выполнением действий, описанных в этой статье, [Переместите выделенный кластер](move-cluster-across-regions.md) в **целевой регион** . Вы также можете использовать шаблон быстрого запуска [на сайте GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) , чтобы создать кластер концентраторов событий. В шаблоне удалите часть пространства имен JSON, чтобы создать только кластер. 

## <a name="prepare"></a>Подготовка
Чтобы приступить к работе, экспортируйте шаблон диспетчер ресурсов. Этот шаблон содержит параметры, описывающие пространство имен концентраторов событий.

1. Войдите на [портал Azure](https://portal.azure.com).
2. Выберите **все ресурсы** , а затем выберите пространство имен концентраторов событий.
3. Выберите **Параметры**>  >  **Экспорт шаблона**.
4. На странице **Экспорт шаблона** нажмите кнопку **загрузить** .

    ![Загрузить шаблон диспетчер ресурсов](./media/move-across-regions/download-template.png)
5. Выберите ZIP-файл, скачанный с портала, и распакуйте его в выбранную папку.

   Этот ZIP-файл содержит JSON-файлы, включающие шаблон и скрипты для развертывания шаблона.


## <a name="move"></a>Переместить

Разверните шаблон, чтобы создать пространство имен концентраторов событий в целевом регионе. 


1. В портал Azure выберите **создать ресурс**.
2. В **поле Поиск в Marketplace** введите **шаблон развертывание** и выберите **шаблоны развертывания (развертывание с помощью пользовательских шаблонов)**.
5. Выберите **Создать собственный шаблон в редакторе**.
6. Выберите **загрузить файл** и следуйте инструкциям по загрузке **template.jsв** файл, скачанный в предыдущем разделе.
1. Обновите значение `location` свойства, чтобы оно указывало на новый регион. Чтобы получить коды расположения, см. раздел [расположения Azure](https://azure.microsoft.com/global-infrastructure/locations/). Код для региона — это имя региона без пробелов, например, `West US` равно `westus` .
1. Нажмите кнопку **сохранить** , чтобы сохранить шаблон. 
1. На странице **Настраиваемое развертывание** выполните следующие действия. 
    1. Выберите **подписку** Azure. 
    2. Выберите существующую **группу ресурсов** или создайте новую. Если исходное пространство имен находилось в кластере концентраторов событий, выберите группу ресурсов, содержащую кластер, в целевом регионе. 
    3. Выберите целевое **Расположение** или регион. Если выбрана существующая группа ресурсов, этот параметр доступен только для чтения. 
    4. В разделе **Параметры** выполните следующие действия.    
        1. Введите новое **имя пространства имен**. 

            ![Развертывание шаблона диспетчер ресурсов](./media/move-across-regions/deploy-template.png)
        2. Если исходное пространство имен находилось в **кластере концентраторов событий**, введите имена **группы ресурсов** и **кластера концентраторов событий** в составе **внешнего идентификатора**. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Если концентратор событий в пространстве имен использует учетную запись хранения для записи событий, укажите имя группы ресурсов и учетную запись хранения для `StorageAccounts_<original storage account name>_external` поля. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. В нижней части страницы выберите **Review + create** (Проверить и создать). 
    1. На странице **Проверка и создание** проверьте параметры и нажмите кнопку **создать**.   

## <a name="discard-or-clean-up"></a>Отмена или очистка
Если после развертывания вы хотите начать заново, можно удалить **целевое пространство имен концентраторов событий** и повторить действия, описанные в разделах [Подготовка](#prepare) и [Перемещение](#move) этой статьи.

Чтобы зафиксировать изменения и завершить перемещение пространства имен концентраторов событий, удалите **пространство имен концентраторов событий** в исходном регионе. Прежде чем удалять пространство имен, убедитесь, что обработаны все события в пространстве имен. 

Чтобы удалить пространство имен концентраторов событий (источник или цель) с помощью портал Azure:

1. В окне поиска в верхней части портал Azure введите **концентраторы событий** и выберите **концентраторы событий** из результатов поиска. В списке отображаются пространства имен концентраторов событий.
2. Выберите целевое пространство имен для удаления и щелкните **Удалить** на панели инструментов. 

    ![Кнопка "удалить пространство имен"](./media/move-across-regions/delete-namespace-button.png)
3. На странице **Удаление пространства имен** подтвердите удаление, введя **имя пространства имен**, а затем выберите **Удалить**. 

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы переместили пространство имен концентраторов событий Azure из одного региона в другой и очистили исходные ресурсы.  Дополнительные сведения о перемещении ресурсов между регионами и аварийном восстановлении в Azure см. по следующей ссылке:


- [Перемещение ресурсов в новую группу ресурсов или подписку](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Перенос виртуальных машин Azure в другой регион](../site-recovery/azure-to-azure-tutorial-migrate.md)
