---
title: Событие завершения изменения размера пула пакетной службы Azure
description: Справочник по событию завершения изменения размера пула пакетной службы. См. пример пула, размер которого увеличился и событие изменения успешно завершено.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: 94301f29fb6e7968dbe0389754fcf2a3b105d7ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "83723822"
---
# <a name="pool-resize-complete-event"></a>Событие завершения изменения размера пула

 Это событие создается при завершении или сбое операции изменения размера пула.

 Следующий пример показывает текст для события завершения изменения размера пула, когда размер пула успешно увеличивается.

```
{
    "id": "myPool",
    "nodeDeallocationOption": "invalid",
        "currentDedicatedNodes": 10,
        "targetDedicatedNodes": 10,
    "currentLowPriorityNodes": 5,
        "targetLowPriorityNodes": 5,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "resultCode": "Success",
    "resultMessage": "The operation succeeded"
}
```

|Элемент|Тип|Примечания|
|-------------|----------|-----------|
|`id`|Строка|Идентификатор пула.|
|`nodeDeallocationOption`|Строка|Указывает, когда можно удалить узлы из пула, если его размер уменьшается.<br /><br /> Возможны следующие значения:<br /><br /> **requeue** — завершает выполняющиеся задачи и повторно ставит их в очередь. Задачи будут запущены повторно при включении задания. Узлы удаляются сразу после завершения задач.<br /><br /> **terminate** — завершает выполняющиеся задачи. Задачи не будут запущены повторно. Узлы удаляются сразу после завершения задач.<br /><br /> **taskcompletion** — разрешает завершить текущие выполняющиеся задачи. Во время ожидания новые задачи не планируются. Узлы удаляются после выполнения всех задач.<br /><br /> **Retaineddata** — разрешает завершить текущие выполняющиеся задачи и затем ожидает, пока истекут сроки хранения данных для всех задач. Во время ожидания новые задачи не планируются. Узлы удаляются после истечения сроков хранения для всех задач.<br /><br /> По умолчанию используется значение requeue.<br /><br /> Если размер пула увеличивается, устанавливается значение **invalid**.|
|`currentDedicatedNodes`|Int32|Число выделенных вычислительных узлов, назначенных пулу.|
|`targetDedicatedNodes`|Int32|Число выделенных вычислительных узлов, запрошенных для пула.|
|`currentLowPriorityNodes`|Int32|Число вычислительных узлов с низким приоритетом, которые в настоящее время назначены пулу.|
|`targetLowPriorityNodes`|Int32|Число вычислительных узлов с низким приоритетом, запрошенных для пула.|
|`enableAutoScale`|Bool|Указывает, корректируется ли размер пула автоматически с течением времени.|
|`isAutoPool`|Bool|Определяет, создан ли пул с помощью механизма AutoPool задания.|
|`startTime`|Дата и время|Время, когда было начато изменение размера пула.|
|`endTime`|Дата и время|Время, когда изменение размера пула было завершено.|
|`resultCode`|Строка|Результат изменения размера.|
|`resultMessage`|Строка| Подробное сообщение о результате.<br /><br /> Если размер успешно изменен, сообщается, что операция выполнена.|
