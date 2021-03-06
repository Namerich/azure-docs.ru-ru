---
title: Подключаемый модуль собственного реагирования для Application Insights SDK JavaScript
description: Установка и использование подключаемого модуля собственного реагирования для Application Insights пакета SDK для JavaScript.
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/06/2020
ms.openlocfilehash: 3dac47aa7cf93ca882c4c1d0d191dabf0eb7178c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88227437"
---
# <a name="native-react-plugin-for-application-insights-javascript-sdk"></a>Подключаемый модуль собственного реагирования для Application Insights SDK JavaScript

Подключаемый модуль реакции на машинный код для Application Insights SDK для JavaScript собирает сведения об устройстве. по умолчанию этот подключаемый модуль автоматически собирает:

- **Уникальный идентификатор устройства** (также известный как идентификатор установки).
- **Имя модели устройства** (например, iPhone X, Samsung Galaxy, Huawei P30 Pro и т. д.)
- **Тип устройства** (например, телефонная трубка, планшетный ПК и т. д.)

## <a name="requirements"></a>Требования

Необходимо использовать версию >= 2.0.0 `@microsoft/applicationinsights-web` . Этот подключаемый модуль будет работать только в собственных приложениях с реагированием. Он не будет работать с [приложениями, использующими платформу экспозиции](https://docs.expo.io/), поэтому он не будет работать с приложением создания и реагировать на собственные приложения.

## <a name="getting-started"></a>Начало работы

Установите и свяжите пакет с [реагированием на устройство](https://www.npmjs.com/package/react-native-device-info) . Обновите `react-native-device-info` пакет, чтобы получить последние имена устройств с помощью приложения.

```zsh

npm install --save @microsoft/applicationinsights-react-native @microsoft/applicationinsights-web
npm install --save react-native-device-info
react-native link react-native-device-info

```

## <a name="initializing-the-plugin"></a>Инициализация подключаемого модуля

Чтобы использовать этот подключаемый модуль, необходимо создать подключаемый модуль и добавить его в качестве `extension` в существующий экземпляр Application Insights.

```typescript
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { ReactNativePlugin } from '@microsoft/applicationinsights-react-native';

var RNPlugin = new ReactNativePlugin();
var appInsights = new ApplicationInsights({
    config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [RNPlugin]
    }
});
appInsights.loadAppInsights();

```

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о пакете SDK для JavaScript см. в [документации по Application Insights JavaScript SDK](javascript.md).
- Дополнительные сведения о языке запросов Kusto и запросе данных в Log Analytics см. в разделе [Общие сведения о запросе журнала](../../azure-monitor/log-query/log-query-overview.md).
