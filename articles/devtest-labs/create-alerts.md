---
title: Создание оповещений журнала действий для лабораторий в Azure DevTest Labs
description: В этой статье описаны действия по созданию оповещений журнала действий для лаборатории в Azure DevTest Labs.
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: d5886ea26ddbeb07efc23d61d3197860620eebf3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90526363"
---
# <a name="create-activity-log-alerts-for-labs-in-azure-devtest-labs"></a>Создание оповещений журнала действий для лабораторий в Azure DevTest Labs
В этой статье объясняется, как создавать оповещения журнала действий для лабораторий в Azure DevTest Labs (например, при создании виртуальной машины или при удалении виртуальной машины).

## <a name="create-alerts"></a>Создание оповещений
В этом примере вы создадите оповещение для всех административных операций в лаборатории с действием, которое отправляет сообщение электронной почты владельцам подписки. 

1. Войдите на [портал Azure](https://portal.azure.com).
1. В строке поиска портал Azure введите **Monitor**и выберите **Monitor** из списка результатов. 

    :::image type="content" source="./media/activity-logs/search-monitor.png" alt-text="Поиск монитора":::        
1. Выберите **оповещения** в меню слева, а затем на панели инструментов выберите **новое правило генерации оповещений** . 

    :::image type="content" source="./media/activity-logs/alerts-page.png" alt-text="Поиск монитора":::    
1. На странице **Создание правила генерации оповещений** щелкните **выбрать ресурс**. 

    :::image type="content" source="./media/activity-logs/select-resource-link.png" alt-text="Поиск монитора":::        
1. Выберите **DevTest Labs** для **фильтра по типу ресурса**, выберите свою лабораторию в списке и нажмите кнопку **Готово**.

    :::image type="content" source="./media/activity-logs/select-lab-resource.png" alt-text="Поиск монитора":::
1. Вернитесь на страницу **Создание правила оповещения** и нажмите кнопку **Выбрать условие**. 

    :::image type="content" source="./media/activity-logs/select-condition-link.png" alt-text="Поиск монитора":::    
1. На странице **Настройка логики сигнала** выберите сигнал, поддерживаемый DevTest Labs. 

    :::image type="content" source="./media/activity-logs/select-signal.png" alt-text="Поиск монитора":::
1. Фильтрация по **уровню события** (verbose, информационное, предупреждение, ошибка, критическая, все), **состояние** (ошибка, запущено, успешно) и **кто инициировал** событие. 
1. Нажмите кнопку **Готово** , чтобы завершить настройку условия. 

    :::image type="content" source="./media/activity-logs/configure-signal-logic-done.png" alt-text="Поиск монитора":::
1. Вы указали для области (Лаборатория) и условие для предупреждения. Теперь необходимо указать группу действий с действиями, которые будут выполняться при выполнении условия. Вернитесь на страницу **Создание правила оповещения** и выберите **пункт выбрать группу действий**. 

    :::image type="content" source="./media/activity-logs/select-action-group-link.png" alt-text="Поиск монитора":::
1. На панели инструментов щелкните ссылку **создать группу действий** . 

    :::image type="content" source="./media/activity-logs/create-action-group-link.png" alt-text="Поиск монитора":::
1. На странице **Добавление группы действий** выполните следующие действия.
    1. Введите **имя** для группы действий.
    1. Введите **короткое имя** для группы действий. 
    1. Выберите **группу ресурсов** , в которой нужно создать оповещение. 
    1. Введите **имя действия**. 
    1. Выберите **тип действия** (в этом примере — **роль Azure Resource Manager электронной почты**). 

        :::image type="content" source="./media/activity-logs/add-action-group.png" alt-text="Поиск монитора":::
    1. На странице **Azure Resource Manager роль электронной почты** выберите роль. В этом примере это **владелец**. Нажмите кнопку **ОК**. 

        :::image type="content" source="./media/activity-logs/select-role.png" alt-text="Поиск монитора":::            
    1. Нажмите **ОК** на странице **Добавить группу действий**. 
1. Теперь на странице **Создание правила оповещения** введите имя для правила оповещения и нажмите кнопку **ОК**. 

    :::image type="content" source="./media/activity-logs/create-alert-rule-done.png" alt-text="Поиск монитора":::

## <a name="view-alerts"></a>Просмотр оповещений 
1. Вы увидите оповещения о **предупреждениях** для всех административных операций (в этом примере это). Отображение оповещений может занять некоторое время. 

    :::image type="content" source="./media/activity-logs/alerts.png" alt-text="Поиск монитора":::
1. Если выбрать число в столбце (например, **всего предупреждений**), отобразятся созданные оповещения. 

    :::image type="content" source="./media/activity-logs/all-alerts.png" alt-text="Поиск монитора":::
1. Если вы выберете оповещение, отобразятся сведения о нем. 

    :::image type="content" source="./media/activity-logs/alert-details.png" alt-text="Поиск монитора":::
1. В этом примере вы также получаете сообщение электронной почты с содержимым, как показано в следующем примере: 

    :::image type="content" source="./media/activity-logs/alert-email.png" alt-text="Поиск монитора":::

## <a name="next-steps"></a>Дальнейшие шаги
- Дополнительные сведения о создании групп действий с помощью различных типов действий см. в разделе [Создание групп действий и управление ими в портал Azure](../azure-monitor/platform/action-groups.md).
- Дополнительные сведения о журналах действий см. в статье  [Журнал действий Azure](../azure-monitor/platform/activity-log.md).
- Дополнительные сведения о настройке оповещений в журналах действий см. в разделе [оповещения в журнале действий](../azure-monitor/platform/activity-log-alerts.md).

