---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: d74279c9c4d5d88e5837046e9484f5af7aad1442
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96001206"
---
Параметр `ApplicationInsights` позволяет добавить в контейнер поддержку телеметрии [Azure Application Insights](/azure/application-insights). Служба Application Insights обеспечивает детализированный мониторинг контейнера. Вы можете легко отслеживать доступность, производительность и использование своего контейнера. Вы также можете быстро идентифицировать и диагностировать ошибки в контейнере.

В следующей таблице описаны параметры конфигурации, поддерживаемые в разделе `ApplicationInsights`.

|Обязательно| Имя | Тип данных | Описание |
|--|------|-----------|-------------|
|нет| `InstrumentationKey` | Строка | Ключ инструментирования экземпляра Application Insights, которому отправляются данные телеметрии для контейнера. Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](../articles/azure-monitor/app/asp-net-core.md). <br><br>Пример<br>`InstrumentationKey=123456789`|