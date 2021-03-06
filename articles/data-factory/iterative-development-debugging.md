---
title: Итеративная разработка и отладка в фабрике данных Azure
description: Узнайте, как итеративно разрабатывать и отлаживать конвейеры фабрики данных в интерфейсе ADF
ms.date: 10/29/2020
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
author: dcstwh
ms.author: weetok
ms.openlocfilehash: 9b28fb24439354e09e5262281a99cd9dc0153a04
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96485252"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Итеративные разработка и отладка в фабрике данных Azure
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Фабрика данных Azure позволяет итеративно разрабатывать и отлаживать конвейеры фабрики данных при разработке решений для интеграции данных. Эти функции позволяют тестировать изменения перед созданием запроса на вытягивание или их публикацией в службе фабрики данных. 

Общие сведения об этой функции и ее демонстрацию см. в следующем 8-минутном видео:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="debugging-a-pipeline"></a>Отладка конвейера

При создании с помощью холста конвейера можно протестировать действия с помощью функции **отладки** . До выбора команды **Отладка** при выполнении тестовых запусков не нужно публиковать изменения в фабрике данных. Это помогает в сценариях, когда перед обновлением рабочих процессов фабрики данных необходимо убедиться, что изменения работают, как и ожидалось.

![Возможность отладки на холсте конвейера](media/iterative-development-debugging/iterative-development-1.png)

По мере выполнения конвейера можно просмотреть результаты каждого действия на вкладке **выходные данные** на холсте конвейера.

Просмотреть результаты тестовых запусков можно в окне **Выходные данные** на холсте конвейера.

![Окно выходных данных на холсте конвейера](media/iterative-development-debugging/iterative-development-2.png)

После успешных тестовых запусков добавьте дополнительные действия в конвейер и продолжайте отладку итеративным методом. В ходе выполнения тестового запуска также можно нажать кнопку **Отмена**.

> [!IMPORTANT]
> Если выбрать функцию **Отладка**, конвейер будет запущен. Например, если конвейер содержит действие копирования, то тестовый запуск копирует данные из источника в место назначения. В итоге в действиях копирования и других действиях при отладке рекомендуется использовать тестовые папки. После отладки конвейера переключитесь на фактические папки, которые нужно использовать во время обычной работы.

### <a name="setting-breakpoints"></a>Задание точек останова

Фабрика данных Azure позволяет выполнять отладку конвейера до тех пор, пока не будет достигнуто определенное действие на холсте конвейера. Установите точку останова на действии, до которого нужно протестировать, и выберите **Отладка**. Фабрика данных обеспечит выполнение теста до указанной точки останова на холсте конвейера. Функцию *Debug Until* (Отладка до момента) следует использовать, когда необходимо проверить не весь конвейер, а только подмножество действий внутри него.

![Точки останова на холсте конвейера](media/iterative-development-debugging/iterative-development-3.png)

Чтобы задать точку останова, выберите элемент на холсте конвейера. Параметр *Отладка до момента* появляется в виде пустого красного круга в правом верхнем углу элемента.

![До включения точки останова для выбранного элемента](media/iterative-development-debugging/iterative-development-4.png)

Когда вы выберете параметр *Отладка до момента*, он изменится на красный круг с заливкой, указывая на включение точки останова.

![После включения точки останова для выбранного элемента](media/iterative-development-debugging/iterative-development-5.png)

## <a name="monitoring-debug-runs"></a>Мониторинг запусков отладки

При выполнении отладки конвейера результаты будут отображаться в окне **вывод** на холсте конвейера. На вкладке выходные данные будет содержаться только Последнее выполнение, произошедшее во время текущего сеанса браузера. 

![Окно выходных данных на холсте конвейера](media/iterative-development-debugging/iterative-development-2.png)

Для просмотра журнала запусков отладки или просмотра списка всех активных запусков отладки можно перейти к **монитору** . 

![Щелкните значок просмотра активных запусков отладки](media/iterative-development-debugging/view-debug-runs.png)

> [!NOTE]
> Служба фабрики данных Azure сохраняет журнал выполнения отладки только в течение 15 дней. 

## <a name="debugging-mapping-data-flows"></a>Отладка потоков данных сопоставления

Сопоставление потоков данных позволяет создавать логику преобразования данных без кода, которая выполняется в масштабе. При построении логики можно включить сеанс отладки для интерактивной работы с данными с помощью динамического кластера Spark. Дополнительные сведения см. в статье [сопоставление режима отладки потока данных](concepts-data-flow-debug-mode.md).

Вы можете отслеживать активные сеансы отладки потока данных по фабрике в процессе **мониторинга** .

![Просмотр сеансов отладки потока данных](media/iterative-development-debugging/view-dataflow-debug-sessions.png)
 
### <a name="debugging-a-pipeline-with-a-data-flow-activity"></a>Отладка конвейера с действием потока данных

При выполнении конвейера отладки, выполняемого с потоком данных, существует два варианта использования вычислений. Можно либо использовать существующий кластер отладки, либо запустить новый JIT-кластер для потоков данных.

Использование существующего сеанса отладки значительно сокращает время запуска потока данных, так как кластер уже выполняется, но не рекомендуется для сложных или параллельных рабочих нагрузок, так как может произойти сбой при одновременном запуске нескольких заданий.

При использовании среды выполнения действий будет создан новый кластер с параметрами, заданными в каждой среде выполнения интеграции действия потока данных. Это позволяет изолировать каждое задание и использовать его для сложных рабочих нагрузок или тестирования производительности. Можно также управлять TTL в Azure IR, чтобы ресурсы кластера, используемые для отладки, по-прежнему были доступны в течение этого периода времени для обслуживания дополнительных запросов заданий.

> [!NOTE]
> При наличии конвейера с потоками данных, выполняемыми параллельно, выберите "использовать среду выполнения действий", чтобы фабрика данных могла использовать Integration Runtime, выбранные в действии потока данных. Это позволит потокам данных выполняться на нескольких кластерах и выполнять параллельные выполнения потоков данных.

![Выполнение конвейера с потоком данных](media/iterative-development-debugging/iterative-development-dataflow.png)

## <a name="next-steps"></a>Дальнейшие действия

После тестирования изменений передвигайте их в более высокие среды, используя [непрерывную интеграцию и развертывание в фабрике данных Azure](continuous-integration-deployment.md).
