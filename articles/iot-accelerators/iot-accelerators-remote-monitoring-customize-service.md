---
title: Добавление службы в пользовательский интерфейс решения для удаленного мониторинга в Azure | Документация Майкрософт
description: В этой статье показано, как добавить новую службу в веб-интерфейс акселератора решения для удаленного мониторинга.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.custom: devx-track-js
ms.openlocfilehash: 6689a6d8a19dd690aa19aeecd29ead35ef6c2db1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91318353"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Добавление пользовательской службы в веб-интерфейс акселератора решения для удаленного мониторинга

В этой статье показано, как добавить новую службу в веб-интерфейс акселератора решения для удаленного мониторинга. Содержание статьи:

- Подготовка локальной среды разработки.
- Добавление новой службы в веб-интерфейс.

Пример службы в этой статье предоставляет данные для сетки, добавление которой описано в статье [Add a custom grid to the Remote Monitoring solution accelerator web UI](iot-accelerators-remote-monitoring-customize-grid.md) (Добавление пользовательской сетки в веб-интерфейс акселератора решения для удаленного мониторинга).

В приложении реагирования служба обычно взаимодействует с внутренней службой. Примеры в акселераторе решения для удаленного мониторинга включают службы, взаимодействующие с диспетчером Центра Интернета вещей и микрослужбами конфигурации.

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, описанные в этом руководстве, необходимо установить следующее ПО на локальный компьютер разработки:

- [Git](https://git-scm.com/downloads);
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Перед началом работы

Прежде чем продолжить, следует выполнить действия, описанные в статье [Добавление пользовательской страницы в веб-интерфейс акселератора решения для удаленного мониторинга](iot-accelerators-remote-monitoring-customize-page.md).

## <a name="add-a-service"></a>Добавление службы

Чтобы добавить службу в веб-интерфейс, необходимо добавить исходные файлы, которые определяют эту службу. Необходимо также изменить некоторые имеющиеся файлы, чтобы веб-интерфейс учитывал новую службу.

### <a name="add-the-new-files-that-define-the-service"></a>Добавление новых файлов, которые определяют службу

Чтобы приступить к работе, перейдите к папке **src/walkthrough/services**. В ней содержатся четыре файла, которые определяют простую службу:

**exampleService.js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Дополнительные сведения о реализации службы см. в статье [о реактивном программировании](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).

**model/exampleModels.js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

Скопируйте файл **exampleService.js** в папку **src/services** и файл **exampleModels.js** в папку **src/services/models**.

Обновите файл **index.js** в папке **src/services** для экспорта новой службы:

```js
export * from './exampleService';
```

Обновите файл **index.js** в папке **src/services/models** для экспорта новой модели:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Настройка вызовов к службе из хранилища

Чтобы начать работу, просмотрите папку **src/walkthrough/store/reducers**, в которой содержится пример модуля сжатия:

**exampleReducer.js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

Скопируйте **exampleReducer.js** в папку **src/store/reducers**.

Дополнительные сведения о модуле сжатия и **Epics** см. на странице [redux-observable](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>Настройка ПО промежуточного слоя

Чтобы настроить ПО промежуточного слоя, добавьте модуль сжатия в файл **rootReducer.js** в папке **src/store**:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

Добавьте epics в файл **rootEpics.js** в папке **src/store**:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Дальнейшие шаги

Из статьи вы узнали о ресурсах, которые могут помочь вам при настройке или добавлении служб в пользовательском веб-интерфейсе в акселераторе решения для удаленного мониторинга.

Вы определили службу. Теперь необходимо [добавить пользовательскую сетку в веб-интерфейс акселератора решения для удаленного мониторинга](iot-accelerators-remote-monitoring-customize-grid.md), который отображает данные, возвращаемые службой.

Дополнительные сведения об ускорителе решений для удаленного мониторинга см. в разделе [архитектура удаленного мониторинга](iot-accelerators-remote-monitoring-sample-walkthrough.md).
