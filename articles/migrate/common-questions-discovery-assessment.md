---
title: Вопросы об обнаружении, оценке и анализе зависимостей в службе "миграция Azure"
description: Получите ответы на часто задаваемые вопросы об обнаружении, оценке и анализе зависимостей в службе "миграция Azure".
ms.topic: conceptual
ms.date: 06/09/2020
ms.openlocfilehash: cb1696c521f436280177f0263abd66aa2bfed7dc
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92312924"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>Обнаружение, оценка и анализ зависимостей — распространенные вопросы

В этой статье содержатся ответы на часто задаваемые вопросы об обнаружении, оценке и анализе зависимостей в службе "миграция Azure". Если у вас есть другие вопросы, ознакомьтесь с этими ресурсами:

- [Общие вопросы](resources-faq.md) о службе "миграция Azure"
- Вопросы об [устройстве "миграция Azure](common-questions-appliance.md) "
- Вопросы о [миграции сервера](common-questions-server-migration.md)
- Получение ответов на вопросы на [форуме по миграции Azure](https://aka.ms/AzureMigrateForum)


## <a name="what-geographies-are-supported-for-discovery-and-assessment-with-azure-migrate"></a>Какие географические объекты поддерживаются для обнаружения и оценки с помощью службы "миграция Azure"?

Просмотрите список поддерживаемых географических регионов для [общедоступного](migrate-support-matrix.md#supported-geographies-public-cloud) облака и облака для [государственных организаций](migrate-support-matrix.md#supported-geographies-azure-government).


## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Сколько виртуальных машин можно обнаружить с помощью устройства?

Вы можете обнаружить до 10 000 виртуальных машин VMware, до 5 000 виртуальных машин Hyper-V и до 1000 физических серверов с помощью одного устройства. Если у вас больше компьютеров, ознакомьтесь [с размасштабированием оценки Hyper-V](scale-hyper-v-assessment.md), [масштабированием оценки VMware](scale-vmware-assessment.md)или [масштабированием физической оценки серверов](scale-physical-assessment.md).

## <a name="how-do-i-choose-the-assessment-type"></a>Как выбрать тип оценки?

- Используйте **оценку виртуальных машин Azure** , если вы хотите оценить локальные [виртуальные машины VMware](how-to-set-up-appliance-vmware.md), [виртуальные машины Hyper-V](how-to-set-up-appliance-hyper-v.md)и [физические серверы](how-to-set-up-appliance-physical.md) для миграции на виртуальные машины Azure. [Подробнее](concepts-assessment-calculation.md)

- Используйте оценки **решения Azure VMware (AVS)** , если хотите оценить локальные [виртуальные машины VMware](how-to-set-up-appliance-vmware.md) для миграции в [решение Azure VMware (AVS)](../azure-vmware/introduction.md) с помощью этого типа оценки. [Подробнее](concepts-azure-vmware-solution-assessment-calculation.md)

- Вы можете использовать общую группу с виртуальными машинами VMware только для выполнения обоих типов оценок. Обратите внимание, что если вы впервые используете оценки AVS в службе "Миграция Azure", рекомендуется создать новую группу виртуальных машин VMware.
 

## <a name="why-is-performance-data-missing-for-someall-vms-in-my-assessment-report"></a>Почему в отчете об оценке отсутствуют данные производительности для некоторых или всех виртуальных машин?

Если устройство службы "Миграция Azure" не может собрать данные производительности для локальных виртуальных машин, для оценки на основе производительности в экспорте отчета об оценке будет присутствовать запись PercentageOfCoresUtilizedMissing или PercentageOfMemoryUtilizedMissing. Необходимо, чтобы:

- Виртуальные машины были включены на период времени, для которого создается оценка.
- Если вы оцениваете виртуальные машины Hyper-V и для них нет только показателей счетчиков памяти, проверьте, включена ли на этих машинах динамическая память. Сейчас есть известная проблема, из-за которой устройство службы "Миграция Azure" не может собирать данные об использовании памяти для таких виртуальных машин.
- Если все счетчики производительности отсутствуют, убедитесь, что исходящие подключения через порты 443 (HTTPS) разрешены.

Примечание. Если нет данных для любого из счетчиков производительности, функция оценки сервера службы "Миграция Azure": возвращается к выделенным локально ядрам или памяти и рекомендует соответствующий размер виртуальной машины.

## <a name="why-is-the-confidence-rating-of-my-assessment-low"></a>Почему оценка имеет низкий рейтинг достоверности?

Оценка достоверности вычисляется для оценки на основе производительности, основываясь на проценте [доступных точек данных](./concepts-assessment-calculation.md#ratings), необходимых для вычисления оценки. Ниже приведены причины, по которым мог быть получен низкий рейтинг достоверности.

- Среда не была профилирована на период времени, для которого создается оценка. Например, если вы создаете оценку с периодом анализа производительности в одну неделю, нужно подождать как минимум неделю после запуска обнаружения для сбора всех точек данных. Если вы не можете выдержать паузу, измените период производительности на более короткий и пересчитайте оценку.
 
- Функция оценки сервера не сможет собрать данные производительности для некоторых или всех виртуальных машин за период оценки. Убедитесь, что виртуальные машины были включены на период времени оценки, а исходящие подключения через порты 443 разрешены. Для виртуальных машин Hyper-V, если включена динамическая память, счетчики памяти будут отсутствовать, в результате чего будет получен низкий рейтинг достоверности. Пересчитайте оценку, чтобы отразить последние изменения оценки достоверности. 

- Несколько виртуальных машин были созданы после запуска обнаружения в решении "Оценка сервера". Например, вы создаете оценку для журнала производительности за последний месяц, при этом несколько виртуальных машин были созданы всего неделю назад. В этом случает данные о производительности новых машин будут недоступны для всего периода анализа, а рейтинг достоверности будет низким.

[Подробнее](./concepts-assessment-calculation.md#confidence-ratings-performance-based) о рейтинге достоверности.

## <a name="i-cant-see-some-groups-when-i-am-creating-an-azure-vmware-solution-avs-assessment"></a>Я не вижу некоторые группы при создании оценки решения Azure VMware (AVS)

- Оценку для AVS можно выполнять для групп, которые включают только виртуальные машины VMware. Чтобы выполнить оценку для AVS, удалите из группы все виртуальные машины, отличные от VMware.
- Если вы впервые используете оценки AVS в службе "Миграция Azure", рекомендуется создать новую группу виртуальных машин VMware.

## <a name="i-cant-see-some-vm-types-in-azure-government"></a>В Azure для государственных организаций не отображаются типы виртуальных машин

Типы виртуальных машин, поддерживаемые для оценки и миграции, зависят от доступности в расположении Azure для государственных организаций. Вы можете [просматривать и сравнивать](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines) типы виртуальных машин в Azure для государственных организаций.

## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>Размер моей виртуальной машины изменился. Можно ли выполнить оценку еще раз?

Устройство "миграция Azure" непрерывно собирает сведения о локальной среде.  Оценка — это моментальный снимок локальных виртуальных машин на момент времени. При изменении параметров на виртуальной машине, которую необходимо оценить, используйте параметр пересчитать, чтобы обновить оценку с учетом последних изменений.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Разделы справки обнаруживать виртуальные машины в многоклиентской среде?

- **VMware**. Если среда является общей для клиентов и вы не хотите обнаруживать виртуальные машины клиента в подписке другого клиента, создайте VMware vCenter Server учетные данные, которые могут обращаться только к виртуальным машинам, которые нужно обнаружить. Затем используйте эти учетные данные при запуске обнаружения в устройстве "миграция Azure".
- **Hyper-v**: обнаружение использует учетные данные узла Hyper-v. Если виртуальные машины используют один и тот же узел Hyper-V, в настоящее время невозможно разделить обнаружение.  

## <a name="do-i-need-vcenter-server"></a>Требуется vCenter Server?

Да, служба "миграция Azure" требует vCenter Server в среде VMware для выполнения обнаружения. Служба "миграция Azure" не поддерживает обнаружение узлов ESXi, которые не управляются с помощью vCenter Server.

## <a name="what-are-the-sizing-options-in-an-azure-vm-assessment"></a>Каковы параметры изменения размера в оценке виртуальной машины Azure?

При изменении размера в локальной среде служба "миграция Azure" не учитывает данные о производительности виртуальной машины для оценки. Служба "миграция Azure" оценивает размеры виртуальных машин на основе локальной конфигурации. Изменение размера на основе производительности зависит от данных об использовании.

Например, если локальная виртуальная машина имеет четыре ядра и 8 ГБ памяти на 50% использования ЦП и 50% использования памяти:
- При изменении размера в локальной среде будет рекомендован номер SKU виртуальной машины Azure с четырьмя ядрами и 8 ГБ памяти.
- При изменении размера на основе производительности будет рекомендован номер SKU виртуальной машины с двумя ядрами и 4 ГБ памяти, поскольку считается процент использования.

Точно так же размер диска зависит от критериев размера и типа хранилища:
- Если критерии изменения размера основаны на производительности и тип хранения — "автоматически", при определении типа целевого диска ("Стандартный" или "Премиум") служба "миграция Azure" учитывает значения операций ввода-вывода и пропускной способности диска.
- Если критерии изменения размера основаны на производительности, а тип хранилища — Premium, служба "миграция Azure" рекомендует номер SKU диска уровня "Премиум" в зависимости от размера локального диска. Та же логика применяется к размерам диска, если используется локальная среда, а тип хранилища — Standard или Premium.

## <a name="does-performance-history-and-utilization-affect-sizing-in-an-azure-vm-assessment"></a>Влияет ли журнал производительности и использование на размер в оценке виртуальной машины Azure?

Да, журнал производительности и использование влияют на размер в оценке виртуальной машины Azure.

### <a name="performance-history"></a>Журнал производительности

Для изменения размера на основе производительности Служба "миграция Azure" собирает журнал производительности локальных компьютеров, а затем использует его для рекомендуемого размера виртуальной машины и типа диска в Azure:

1. Устройство непрерывно выполняет профилирование локальной среды для сбора данных об использовании в режиме реального времени каждые 20 секунд.
2. Устройство выполняет сведение собранных 20-секундных выборок и использует их для создания одной точки данных каждые 15 минут.
3. Чтобы создать точку данных, устройство выбирает пиковое значение из всех 20-секундных выборок.
4. Устройство отправляет точку данных в Azure.

### <a name="utilization"></a>Использование

При создании оценки в Azure, в зависимости от длительности производительности и заданного значения процентилей, служба "миграция Azure" вычисляет значение эффективного использования, а затем использует его для изменения размера.

Например, если задать длительность производительности равным одному дню, а значение процентиля — 95 процентиль процентиль, то служба "миграция Azure" сортирует 15-минутные точки выборки, отправленные сборщиком за последний день в возрастающем порядке. Он выбирает значение 95 процентиль процентиль в качестве эффективного использования.

Использование значения 95 процентиль процентиль гарантирует, что выбросы не будут учитываться. Выбросы могут быть добавлены, если в службе "миграция Azure" используется 99-м процентиль. Чтобы выбрать пиковое использование для периода без каких бы то ни было выбросов, настройте миграцию Azure на использование 99-м процентиль.


## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>Каким образом оценки на основе импорта отличаются от оценок с источником обнаружения как устройством?

Оценки виртуальных машин Azure на основе импорта — это оценки, созданные на компьютерах, которые импортируются в службу "миграция Azure" с помощью CSV-файла. Для импорта обязательно только четыре поля: имя сервера, ядра, память и операционная система. Обратите внимание на следующее: 
 - Условия готовности менее строги при оценке на основе импорта в параметре типа загрузки. Если тип загрузки не указан, предполагается, что компьютер имеет тип загрузки BIOS, а компьютер не помечен как **условно готовый**. При оценке с источником обнаружения как устройство, готовность помечается как **условно готовая** , если отсутствует тип загрузки. Это различие в вычислении готовности заключается в том, что у пользователей могут отсутствовать все сведения на компьютерах на ранних стадиях планирования миграции, когда выполняются оценки на основе импорта. 
 - При оценке импорта с учетом производительности используется значение использования, предоставленное пользователем для вычислений с использованием правильного размера. Так как значение использования предоставляется пользователем, параметры " **Журнал производительности** " и " **процентиль** " могут быть отключены в свойствах оценки. При оценке с источником обнаружения как устройство, выбранное значение процентиля выбирается из данных о производительности, собираемых устройством.

## <a name="why-is-the-suggested-migration-tool-in-import-based-avs-assessment-marked-as-unknown"></a>Почему предлагаемое средство миграции для оценки AVS на основе импорта помечено как неизвестное?

Для компьютеров, импортированных с помощью CSV-файла, средство миграции по умолчанию в оценке AVS неизвестно. Однако для компьютеров VMware рекомендуется использовать решение гибридного облака VMware (ХККС). [Подробнее](../azure-vmware/tutorial-deploy-vmware-hcx.md)


## <a name="what-is-dependency-visualization"></a>Что такое визуализация зависимостей?

Визуализация зависимостей помогает оценить группы виртуальных машин, которые будут переноситься с большей достоверностью. Визуализация зависимостей перекрестно проверяет зависимости компьютера перед выполнением оценки. Это гарантирует, что ничего не останется, и это помогает избежать непредвиденных простоев при миграции в Azure. Служба "Миграция Azure" использует решение "Сопоставление служб" в Azure Monitor для визуализации зависимостей. [Подробнее](concepts-dependency-visualization.md).

> [!NOTE]
> Анализ зависимостей на основе агента недоступен в Azure для государственных организаций. Можно использовать анализ зависимостей без агента.

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>В чем разница между агентом и без агента?

Различия между визуализацией без агента и визуализацией на основе агентов приведены в таблице.

**Требование** | **Безагентное** | **На основе агентов**
--- | --- | ---
Поддержка | Сейчас этот параметр находится на этапе предварительной версии и доступен только для виртуальных машин VMware. [Ознакомьтесь](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) с поддерживаемыми операционными системами. | В общем доступе (GA).
Агент | Нет необходимости устанавливать агенты на компьютерах, которые требуется перекрестно проверить. | Агенты, которые будут установлены на каждом локальном компьютере, который необходимо проанализировать: [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md)и [Агент зависимостей](../azure-monitor/platform/agents-overview.md#dependency-agent). 
предварительные требования | [Ознакомьтесь](concepts-dependency-visualization.md#agentless-analysis) с требованиями к развертыванию и предварительным требованиям. | [Ознакомьтесь](concepts-dependency-visualization.md#agent-based-analysis) с требованиями к развертыванию и предварительным требованиям.
Log Analytics | Не требуется. | Служба "Миграция Azure" использует решение [Сопоставление служб](../azure-monitor/insights/service-map.md) в [журналах Azure Monitor](../azure-monitor/log-query/log-query-overview.md) для визуализации зависимостей. [Подробнее](concepts-dependency-visualization.md#agent-based-analysis).
Принцип работы | Захватывает данные подключения TCP на компьютерах, включенных для визуализации зависимостей. После обнаружения он собирает данные через пять минут. | Сопоставление служб агенты, установленные на компьютере, собирают данные о TCP-процессах и входящих и исходящих подключениях для каждого процесса.
Данные | Имя сервера исходного компьютера, процесс, имя приложения.<br/><br/> Имя сервера конечного компьютера, процесс, имя приложения и порт. | Имя сервера исходного компьютера, процесс, имя приложения.<br/><br/> Имя сервера конечного компьютера, процесс, имя приложения и порт.<br/><br/> Количество соединений, задержка и сведения о передачи данных собираются и доступны для запросов Log Analytics. 
Визуализация | Карту зависимостей отдельного сервера можно просмотреть в течение одного часа до 30 дней. | Схема зависимостей одного сервера.<br/><br/> Карту можно просматривать только в течение часа.<br/><br/> Схема зависимостей группы серверов.<br/><br/> Добавление и удаление серверов в группе из представления карт.
Экспорт данных | Данные за последние 30 дней можно скачать в формате CSV. | Данные можно запрашивать с помощью Log Analytics.


## <a name="do-i-need-to-deploy-the-appliance-for-agentless-dependency-analysis"></a>Нужно ли развертывать устройство для анализа зависимостей без агента?

Да, [устройство для миграции Azure](migrate-appliance.md) должно быть развернуто.

## <a name="do-i-pay-for-dependency-visualization"></a>Взимается ли плата за визуализацию зависимостей?

Нет. Дополнительные сведения о [ценах на миграцию Azure](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Что следует установить для визуализации зависимостей на основе агентов?

Чтобы использовать визуализацию зависимостей на основе агента, скачайте и установите агенты на каждом локальном компьютере, который необходимо оценить.

- [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md)
- [Агент зависимостей](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Если у вас есть компьютеры без подключения к Интернету, скачайте и установите на них шлюз Log Analytics.

Эти агенты необходимы, только если используется Визуализация зависимостей на основе агента.

## <a name="can-i-use-an-existing-workspace"></a>Можно ли использовать имеющуюся рабочую область?

Да, для визуализации зависимостей на основе агентов можно подключить существующую рабочую область к проекту миграции и использовать ее для визуализации зависимостей. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Можно ли экспортировать отчет о визуализации зависимостей?

Нет, экспорт отчета по визуализации зависимостей в визуализации на основе агента невозможен. Однако служба "миграция Azure" использует Сопоставление служб, и для получения зависимостей в формате JSON можно использовать [REST API сопоставление служб](/rest/api/servicemap/machines/listconnections) .

## <a name="can-i-automate-agent-installation"></a>Можно ли автоматизировать установку агента?

Для визуализации зависимостей на основе агента:

- Используйте [скрипт для установки агента зависимостей](../azure-monitor/insights/vminsights-enable-hybrid.md#dependency-agent).
- Для MMA [используйте командную строку или автоматизацию](../azure-monitor/platform/log-analytics-agent.md#installation-options)или используйте [Скрипт](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).
- В дополнение к сценариям для развертывания агентов можно использовать средства развертывания, такие как Microsoft Endpoint Configuration Manager и [Intigua](https://www.intigua.com/intigua-for-azure-migration) .

## <a name="what-operating-systems-does-mma-support"></a>Какие операционные системы поддерживает MMA?

- Просмотрите список [операционных систем Windows, ПОДДЕРЖИВАЕМЫХ MMA](../azure-monitor/platform/log-analytics-agent.md#installation-options).
- Просмотрите список [операционных систем Linux, ПОДДЕРЖИВАЕМЫХ MMA](../azure-monitor/platform/log-analytics-agent.md#installation-options).

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>Можно ли визуализировать зависимости более одного часа?

Для визуализации на основе агентов можно визуализировать зависимости до одного часа. Вы можете вернуться к определенной дате в журнале в течение одного месяца, но максимальная продолжительность визуализации — один час. Например, можно использовать длительность времени на карте зависимостей для просмотра зависимостей за вчерашний период, но можно просмотреть зависимости только в течение одного часового окна. Однако можно использовать журналы Azure Monitor для [запроса данных зависимости](./how-to-create-group-machine-dependencies.md) в течение более длительного времени.

Для визуализации без агента можно просмотреть карту зависимостей одного сервера в интервале от 1 часа до 30 дней.

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>Можно ли визуализировать зависимости для групп с более чем 10 виртуальными машинами?

Можно [визуализировать зависимости](./how-to-create-a-group.md#refine-a-group-with-dependency-mapping) для групп, имеющих до 10 виртуальных машин. Если у вас есть группа, которая содержит более 10 виртуальных машин, рекомендуется разбить группу на группы меньшего размера, а затем визуализировать зависимости.

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [обзором службы "миграция Azure](migrate-services-overview.md)".