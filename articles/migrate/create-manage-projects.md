---
title: Создание и администрирование проектов Миграции Azure
description: Поиск, создание, управление и удаление проектов в службе "миграция Azure".
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: c352c06a5e5b798563b4543122f66a302017bc8a
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500841"
---
# <a name="create-and-manage-azure-migrate-projects"></a>Создание и администрирование проектов Миграции Azure

В этой статье описывается создание, Администрирование и удаление проектов службы " [Миграция Azure](migrate-services-overview.md) ".

Проект Миграции Azure используется для хранения метаданных обнаружения, оценки и миграции, собранных из среды, для которой выполняется оценка или миграция. В проекте можно контролировать обнаруженные ресурсы, создавать оценки и управлять миграцией в Azure.  

## <a name="verify-permissions"></a>Проверка разрешений

Убедитесь, что у вас есть необходимые разрешения для создания проекта службы "миграция Azure":

1. В портал Azure откройте соответствующую подписку и выберите **Управление доступом (IAM)**.
2. На странице **Проверка доступа** найдите соответствующую учетную запись и выберите пункт Просмотр разрешений. Вы должны обладать разрешениями уровня *Участник* или *Владелец*. 


## <a name="create-a-project-for-the-first-time"></a>Создание проекта в первый раз

Настройте новый проект службы "миграция Azure" в подписке Azure.

1. В портал Azure выполните поиск по запросу служба " *Миграция Azure*".
2. В списке **службы** выберите **Миграция Azure**.
3. В разделе **Обзор** щелкните **Оценка и миграция серверов**.

    ![Параметр в разделе "Обзор" для оценки и миграции серверов](./media/create-manage-projects/assess-migrate-servers.png)

4. На панели **серверы** выберите **создать проект**.

    ![Кнопка для начала создания проекта](./media/create-manage-projects/create-project.png)

5. В окне **Создание проекта** выберите подписку Azure и группу ресурсов. Если у вас еще нет группы ресурсов, создайте ее.
6. В разделе **Сведения о проекте** укажите имя проекта и регион для создания проекта.
    - География используется только для хранения метаданных, собранных с локальных компьютеров. Можно выбрать любой целевой регион для миграции. 
    - Просмотрите список поддерживаемых регионов для [общедоступного](migrate-support-matrix.md#supported-geographies-public-cloud) облака и облака для [государственных организаций](migrate-support-matrix.md#supported-geographies-azure-government).

8. Нажмите кнопку **создания**.

   ![Страница с параметрами входного проекта](./media/create-manage-projects/project-details.png)


Развертывание проекта службы "Миграция Azure" может занять несколько минут.

## <a name="create-a-project-in-a-specific-region"></a>Создание проекта в определенном регионе

На портале можно выбрать географию, в котором будет создан проект. Если вы хотите создать проект в определенном регионе Azure, используйте следующую команду API для создания проекта.

```rest
PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"
```

## <a name="create-additional-projects"></a>Создание дополнительных проектов

Если у вас уже есть проект службы "миграция Azure" и вы хотите создать дополнительный проект, выполните следующие действия.  

1. На [общедоступном портале Azure](https://portal.azure.com) или в [Azure для государственных организаций](https://portal.azure.us)найдите службу " **Миграция Azure**".
2. На панели мониторинга миграция Azure > **серверы** щелкните **изменить** в правом верхнем углу.

   ![Изменение проекта службы "миграция Azure"](./media/create-manage-projects/switch-project.png)

3. Чтобы создать новый проект, выберите **щелкните здесь**.


## <a name="find-a-project"></a>Найти проект

Найдите проект следующим образом:

1. В [портал Azure](https://portal.azure.com)выполните поиск по запросу служба " *Миграция Azure*".
2. На панели мониторинга миграция Azure > **серверы** щелкните **изменить** в правом верхнем углу.

    ![Переключение на существующий проект службы "миграция Azure"](./media/create-manage-projects/switch-project.png)

3. Выберите подходящую подписку и проект "миграция Azure".


### <a name="find-a-legacy-project"></a>Поиск проекта прежних версий

Если вы создали проект в [предыдущей версии](migrate-services-overview.md#azure-migrate-versions) службы "миграция Azure", найдите его следующим образом:

1. В [портал Azure](https://portal.azure.com)выполните поиск по запросу служба " *Миграция Azure*".
2. Если вы создали проект в предыдущей версии, на панели мониторинга "миграция Azure" появится баннер, ссылающаяся на старые проекты. Выберите баннер.

    ![Доступ к существующим проектам](./media/create-manage-projects/access-existing-projects.png)

3. Проверьте список старых проектов.


## <a name="delete-a-project"></a>Удаление проекта

Удаление выполняется следующим образом:

1. Откройте группу ресурсов Azure, в которой был создан проект.
2. На странице Группа ресурсов выберите пункт **Показывать скрытые типы**.
3. Выберите проект миграции, который необходимо удалить, и связанные с ним ресурсы.
    - Тип ресурса — **Microsoft. Migrate/мигратепрожектс**.
    - Если группа ресурсов используется исключительно в проекте службы "миграция Azure", можно удалить всю группу ресурсов.

Обратите внимание на следующие условия.

- При удалении проекта и метаданных об обнаруженных компьютерах удаляются.
- Если вы используете более раннюю версию службы "миграция Azure", откройте группу ресурсов Azure, в которой был создан проект. Выберите проект миграции, который необходимо удалить (тип ресурса — **проект миграции**).
- Если вы используете анализ зависимостей в рабочей области Azure Log Analytics:
    - Если вы подключили рабочую область Log Analytics к средству оценки серверов, Рабочая область не удаляется автоматически. Ту же рабочую область Log Analytics можно использовать для нескольких сценариев.
    - Если вы хотите удалить рабочую область Log Analytics, сделайте это вручную.
- Удаление проекта необратимо. Удаленные объекты невозможно восстановить.

### <a name="delete-a-workspace-manually"></a>Удаление рабочей области вручную

1. Перейдите к рабочей области Log Analytics, связанной с проектом.

    - Если вы еще не удалили проект службы "миграция Azure", ссылку на рабочую область можно найти в подокне оценки **Essentials**  >  **Server**.
       ![LA Рабочая область ](./media/create-manage-projects/loganalytics-workspace.png) .
       
    - Если вы уже удалили проект "миграция Azure", выберите **группы ресурсов** в левой области портал Azure и найдите рабочую область.
       
2. [Следуйте инструкциям](../azure-monitor/platform/delete-workspace.md) , чтобы удалить рабочую область.

## <a name="next-steps"></a>Дальнейшие действия

Добавление средств [оценки](how-to-assess.md) или [миграции](how-to-migrate.md) в проекты службы "миграция Azure".
