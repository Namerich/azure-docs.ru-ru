---
title: Настройка оповещений метрик — портал Azure — гибкий сервер в базе данных Azure для MySQL
description: В этой статье описывается, как настроить оповещения метрик и получить доступ к ним для гибкого сервера базы данных Azure для MySQL из портал Azure.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 4a099a9850289a046435b4e1763d7f54a702c0d0
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545095"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql---flexible-server"></a>Используйте портал Azure, чтобы настроить оповещения о метриках для базы данных Azure для MySQL — гибкого сервера. 

> [!IMPORTANT] 
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

В этой статье показано, как настроить оповещения для базы данных Azure для MySQL с помощью портала Azure. Вы можете получать оповещения на основе метрик мониторинга для служб Azure.

Оповещение активируется, когда значение указанной метрики выходит за рамки заданного порога. Сначала оно активируется, когда условие выполняется, а затем — когда условие перестает выполняться. Функция оповещений о метриках работает с отслеживанием состояния, то есть уведомления отправляются только при изменении состояния.

Оповещения можно настроить для выполнения следующих действий при его активации:
* отправка уведомлений по электронной почте администратору службы и соадминистраторам;
* отправка уведомления на указанные дополнительные электронные адреса;
* Вызов webhook.

Для настройки правил генерации оповещений и получения сведений о них можно использовать:
* [Портал Azure](../../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [REST API Azure Monitor](/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Создание правила генерации оповещений на основе метрики на портале Azure
1. В [портал Azure](https://portal.azure.com/)выберите гибкий сервер базы данных Azure для MySQL, который требуется отслеживать.
2. В разделе **мониторинг** боковой панели выберите **оповещения** .
3. Выберите **+ Новое правило генерации оповещений** .
4. Откроется страница **Создание правила** . Заполните необходимые сведения:
5. В разделе **условие** выберите **Выбрать условие** .
6. Вы увидите список поддерживаемых сигналов, выберите метрику, для которой нужно создать оповещение. Например, выберите "процент хранилища".
7. Вы увидите диаграмму для метрики за последние шесть часов. С помощью раскрывающегося списка **период диаграммы** выберите, чтобы просмотреть более длительный журнал для метрики.
8. Выберите тип **порога** (например, "Static" или "Dynamic"), **оператор** (например, "Больше") и **тип агрегирования** (например, среднее значение). Это определит логику, которую будет оценивать правило оповещения метрики.
    - Если используется **статическое** пороговое значение, продолжайте определение **порогового значения** (например, 85 процентов). Диаграмма метрик может помочь определить, что может быть разумным пороговым значением.
    - Если используется **динамическое** пороговое значение, продолжайте определять **чувствительность к пороговому значению** . На диаграмме метрик отображаются вычисленные пороговые значения на основе последних данных. [Дополнительные сведения о типе условия "Динамические пороговые значения" и значениях параметра конфиденциальности](../../azure-monitor/platform/alerts-dynamic-thresholds.md).
9. Уточните условие, изменив интервал **детализации статистической обработки (периода)** , через который группируются точки данных с помощью функции типа агрегирования (например, «30 минут») и « **Частота** » (например, «каждые 15 минут»).
10. Нажмите кнопку **Done** (Готово).
11. Добавление группы действий. Группа действий — это коллекция параметров уведомлений, которые определены владельцем подписки Azure. В разделе **группы действий** выберите **пункт выбрать группу действий** , чтобы выбрать уже существующую группу действий для присоединения к правилу оповещения.
12. Вы также можете создать новую группу действий для получения уведомлений о предупреждении. Дополнительные сведения см. в статье [Создание группы действий и управление ею](../../azure-monitor/platform/action-groups.md) .
13. Чтобы создать новую группу действий, выберите **+ создать группу действий** . Заполните форму "Создание группы действий" с **подпиской** , **группой ресурсов** , **именем группы действий** и **отображаемым именем** .
14. Настройка **уведомлений** для группы действий.
    
    В списке **тип уведомления** выберите "роль электронной почты Azure Resource Manager", чтобы выбрать владельцев, участников и читателей для получения уведомлений. Выберите **роль Azure Resource Manager** для отправки сообщения электронной почты.
    Для отправки уведомлений конкретным получателям можно также выбрать **сообщения электронной почты, SMS, Push/Voice** .

    Укажите **имя** для типа уведомления и выберите **проверить и создать** после завершения.

    <!--:::image type="content" source="./media/howto-alert-on-metric/10-action-group-type.png" alt-text="Action group":::-->
    
15. Введите **сведения о правиле генерации оповещений** , такие как **имя правила оповещения** , **Описание** , **Сохранение правила оповещения в группе ресурсов** и **серьезности** .

    <!--:::image type="content" source="./media/howto-alert-on-metric/11-name-description-severity.png" alt-text="Action group":::-->

16. Выберите **Создать правило генерации оповещений** , чтобы создать оповещение.
    Через несколько минут оповещение включится и будет активироваться, как было описано выше.
## <a name="manage-your-alerts"></a>Управление оповещениями
После создания оповещение можно выбрать и сделать следующее:

* просмотреть диаграмму, отображающую пороговое и фактическое значения метрики за предыдущий день, относящиеся к этому оповещению;
* **изменить** или **удалить** правило генерации оповещений;
* **отключить** или **включить** его, если нужно временно остановить или возобновить получение уведомлений.


## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о [настройке оповещений о метриках](../../azure-monitor/platform/alerts-metric.md).
- Узнайте больше о доступных [метриках в базе данных Azure для гибкого сервера MySQL](./concepts-monitoring.md).
- [Сведения о работе оповещений о метриках в Azure Monitor](../../azure-monitor/platform/alerts-metric-overview.md)