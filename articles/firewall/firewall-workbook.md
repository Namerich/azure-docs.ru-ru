---
title: Мониторинг журналов с помощью книги брандмауэра Azure
description: Книги брандмауэра Azure предоставляют гибкий холст для анализа данных брандмауэра Azure и создания полнофункциональных визуальных отчетов в портал Azure.
services: firewall
author: gopimsft
ms.service: firewall
ms.topic: how-to
ms.date: 09/22/2020
ms.author: victorh
ms.openlocfilehash: dabe154535d2bd3ae5550d4bbbafa75adc5ae2c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91347379"
---
# <a name="monitor-logs-using-azure-firewall-workbook"></a>Мониторинг журналов с помощью книги брандмауэра Azure

Книга брандмауэра Azure предоставляет гибкий холст для анализа данных брандмауэра Azure. Его можно использовать для создания многофункциональных визуальных отчетов в портал Azure. Вы можете коснуться нескольких брандмауэров, развернутых в Azure, и объединить их в единый интерактивный интерфейс.

Вы можете получить представление о событиях брандмауэра Azure, узнать о правилах приложения и сети, а также просмотреть статистику действий брандмауэра по URL-адресам, портам и адресам. Книга брандмауэров Azure позволяет фильтровать брандмауэры и группы ресурсов, а также динамически фильтровать по категориям с помощью простых для чтения наборов данных при исследовании проблем в журналах. 

## <a name="prerequisites"></a>Предварительные требования

Перед началом необходимо [включить ведение журнала диагностики](firewall-diagnostics.md#enable-diagnostic-logging-through-the-azure-portal) с помощью портал Azure. Кроме того, ознакомьтесь с [журналами и метриками брандмауэра Azure](logs-and-metrics.md) , чтобы получить общие сведения о журналах диагностики и метриках, доступных для брандмауэра Azure.

## <a name="get-started"></a>Начало работы

Чтобы развернуть книгу, перейдите в раздел [Azure Monitor книга для брандмауэра Azure](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20Firewall/Azure%20Monitor%20Workbook) и следуйте инструкциям на странице. Книга брандмауэра Azure предназначена для работы с несколькими клиентами, несколькими подписками и поддерживает фильтрацию по нескольким брандмауэрам.

## <a name="overview-page"></a>Страница "Обзор"

На странице Обзор можно выполнить фильтрацию по рабочим областям, времени и брандмауэрам. В нем отображаются события по времени для брандмауэров и типов журналов (приложения, сети, угроза Intel, DNS-прокси).

:::image type="content" source="./media/firewall-workbook/firewall-workbook-overview.png" alt-text="Обзор книги по брандмауэру Azure":::

## <a name="application-rule-log-statistics"></a>Статистика журнала правил приложений

На этой странице показаны уникальные источники IP-адресов с течением времени, количество правил приложения, запрещенные или разрешенные полные доменные имена с временем, а также отфильтрованные данные. Данные можно фильтровать по IP-адресу.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-application-rule.png" alt-text="Обзор книги по брандмауэру Azure":::

## <a name="network-rule-log-statistics"></a>Статистика журнала сетевых правил

На этой странице представлено действие правила — разрешить или запретить, целевой порт по IP-адресу и ДНаТ с течением времени. Можно также выполнять фильтрацию по типу действия, порту и назначению.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-network-rule.png" alt-text="Обзор книги по брандмауэру Azure":::

Можно также фильтровать журналы в зависимости от временного интервала:

:::image type="content" source="./media/firewall-workbook/firewall-workbook-network-rule-time.png" alt-text="Обзор книги по брандмауэру Azure":::

## <a name="investigations"></a>Операций

Вы можете просмотреть журналы и узнать больше о ресурсе на основе IP-адреса источника. Вы можете получить такие сведения, как имя виртуальной машины и имя сетевого интерфейса. Можно просто отфильтровать ресурс из журналов.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-investigation.png" alt-text="Обзор книги по брандмауэру Azure":::

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о [диагностике брандмауэра Azure](firewall-diagnostics.md)