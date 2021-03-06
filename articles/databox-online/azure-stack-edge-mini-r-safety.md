---
title: Azure Stack пограничной мини-руководством по безопасности R | Документация Майкрософт
description: Описываются соглашения о безопасности, рекомендации, рекомендации и объясняется, как безопасно устанавливать и использовать мини-устройство с Azure Stack пограничным устройством R.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: alkohli
ms.openlocfilehash: 507ceef0f13951eafdcb02d586f35c1d61764c4e
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96467474"
---
# <a name="azure-stack-edge-mini-r-safety-instructions"></a>Azure Stack пограничных инструкций по безопасности R

![Значок предупреждения 1 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ чтение уведомления о безопасности значок "чтение ](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png)
 **сведений о безопасности и работоспособности** "

Ознакомьтесь со всеми сведениями о безопасности, приведенными в этой статье, прежде чем использовать мини-устройство с Azure Stack пограничным устройством R, составить один пакет аккумулятора, один подключенный к сети блок питания, один адаптер питания модуля и один серверный модуль. Несоблюдение инструкций может привести к пожару, электрическим ударам, травмаху или повреждению свойств. Прочитайте все сведения о безопасности ниже, прежде чем использовать Azure Stack пограничных Мини-R.

## <a name="safety-icon-conventions"></a>Условные обозначения сведений о безопасности

Ниже перечислены ключевые слова для подписывания предупреждений опасности.

| Значок | Описание |
|:--- |:--- |
| ![Символ опасности](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)| **Опасность:** Обозначает опасную ситуацию, которая, в противном случае, приведет к смерти или серьезной травме. <br> **Предупреждение:** Указывает на опасную ситуацию, которая может привести к смерти или серьезным травмам. <br> **Внимание!** Указывает на опасную ситуацию, которая может привести к незначительному или умеренному травме.|
|

При настройке и запуске мини-устройства R для Azure Stack пограничных устройств можно использовать следующие значки опасности:

| Значок | Описание |
|:--- |:--- |
| ![Сначала прочесть все инструкции](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png) | Сначала прочесть все инструкции |
| ![Символ опасности](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) | Символ опасности |
| ![Значок «Электрошок»](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) | Опасность поражения электрическим током |
| ![Только использование внутри](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) | Только использование внутри |
| ![Значок «Нет компонентов, подлежащих самостоятельному ремонту»](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) | Нет элементов, подобслуживаемых пользователем. Не проводите техническое обслуживание устройства без должной подготовки. |
|

## <a name="handling-precautions-and-site-selection"></a>Обработка мер предосторожности и выбора сайта

На Azure Stack пограничных устройствах R имеется следующая обработка предосторожностей и критериев выбора сайта:

![Значок предупреждения 2 значок ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ электрической электромагнитной сети ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ значок без обслуживаемых элементов ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **Предупреждение:**

* Проверьте состояние устройства *в момент получения*. Если корпус устройства поврежден, обратитесь в [Служба поддержки Майкрософт](azure-stack-edge-placeholder.md) , чтобы получить замену. Не пытайтесь работать с поврежденным устройством.
* Если вы подозреваете, что устройство работает неправильно, [обратитесь в Служба поддержки Майкрософт](azure-stack-edge-placeholder.md) , чтобы получить замену. Не пытайтесь проводить техническое обслуживание самостоятельно.
* Устройство содержит компоненты, которые не подлежат обслуживанию пользователем. Внутри корпуса присутствует опасный уровень напряжения, тока и электроэнергии. Не открывайте корпус. Верните устройство в Майкрософт для проведения обслуживания.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 3 внимание!**

Рекомендуется использовать систему:

* От источников тепла, включая прямые солнечные и аппаратные радиочастоту.
* В расположениях, не предоставляемых влажность или дождя.
* Размещается в пространстве, в котором уменьшается вибрация и снижается физическая нагрузка.  Система разработана для ударов и вибрации в соответствии с MIL-STD-810G.
* Изолировано от сильных электромагнитных полей, созданных электрическими устройствами.
* Не разрешайте системе никаких ликвидных или ни одного внешнего объекта. Не помещайте напитки или другие контейнеры жидкостей в систему или рядом с ней.

![Значок предупреждения 4 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок "обслуживаемые компоненты ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **" отсутствует внимание!**

* Это оборудование содержит литий-аккумулятор. Не пытайтесь обслуживать пакет аккумулятора. Батареи в этом оборудовании не обслуживаются пользователями. Риск развертывания, если батарея заменена неверным типом.

![Значок предупреждения 5 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **. внимание!**

Зарядка пакета аккумулятора только в том случае, если он входит в состав Azure Stack пограничной устройства R, не взимается как отдельное устройство.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 6 внимание!**

* Параметр включения/выключения в пакете аккумулятора предназначен для отключения аккумулятора только серверному модулю. Если адаптер питания подключен к пакету аккумулятора, питание передается в модуль сервера, даже если параметр находится в положении OFF.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 7 внимание!**

* Не записывает и не выкоротких пакетов аккумулятора. Его необходимо перезапустить или удалить должным образом.

![Значок предупреждения 8 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **. внимание!**

* В вместо использования указанного источника питания AC/DC эта система также имеет возможность использовать заданный в поле Тип 2590 аккумулятор. В этом случае конечный пользователь должен убедиться, что он соответствует всем применимым требованиям безопасности, транспорту, окружающей среде и другим нормативным и местным нормам.
* При работе с системой типа "батарея 2590" используйте батарею в условиях использования, заданных производителем батареи.

![Значок предупреждения ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **: 9 внимание!**

Это устройство имеет два порта SFP +, которые можно использовать с оптическими приемопередатчиками. Чтобы избежать опасных лазерных излучений, используйте только с приемопередатчиками класса 1.

## <a name="electrical-precautions"></a>Меры предосторожности в отношении электрооборудования

На Azure Stack пограничном устройстве R есть следующие электрические меры предосторожности:

![Значок предупреждения 10 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) ![ значок электрического удара ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **Предупреждение:**

При использовании с адаптером источника питания:

* Обеспечьте безопасное соединение с землей для кабеля электропитания. Кабель с чередованием (AC) имеет три провода заземления (вилка с заземлением). Эта вилка подходит только к заземленной розетке переменного тока. Не пренебрегайте заземляющим контактом.
* Так как штекер кабеля электропитания является основным средством отключения, убедитесь, что розетки расположены вблизи устройства и в удобном для вас месте.
* Отключите шнуры питания (выполнив подключение через разъем, а не шнур) и Разъедините все кабели, если выполняются следующие условия.

  * Кабель питания и вилка потерты или повреждены любым другим образом.
  * Устройство доступно для дождя, избыточного влажность или другого Liquids.
  * Устройство было удалено, и регистр устройств поврежден.
  * По вашему мнению, устройство требует обслуживания или ремонта.
* Перед перемещением устройства, или если вам кажется, что оно неисправно, полностью отключите его от электросети.

* Используйте подходящий источник питания с защитой от электрической перегрузки, чтобы обеспечить соответствие следующим требованиям:

* Напряжение: 100-240 вольт AC
* Текущий: 1,7 Амперес
* Частота: от 50 до 60 Гц

![Значок предупреждения 11 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок электрического электричества ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **Предупреждение:**

* Не пытайтесь изменить или использовать шнуры питания сети, отличные от устройств, поставляемых с оборудованием.

![Значок предупреждения 12 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ значок электрических ударов ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ . Используйте только ](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) **Предупреждение:**

* Источник питания, обозначенный этим символом, оценивается только для использования.

## <a name="regulatory-information"></a>Сведения о соответствии нормам

Ниже приведены сведения о соответствии нормативным требованиям для устройства Azure Stack пограничных устройств R, номера модели соответствия: TMA01.

Устройство мини R Azure Stack пограничной Организации предназначено для использования с НРТЛ в списке (UL, CSA, ETL и т. д.), а также для оборудования IEC/EN 60950-1 или IEC/EN 62368-1 (отмечено в соответствии с маркировкой CE).

В странах, отличных от США и Канады, сетевые кабели (не поставляемые вместе с оборудованием) не должны устанавливаться вместе с этим оборудованием, если их длина превышает 3 метров.

Оборудование предназначено для использования в следующих средах:

| Среда | Характеристики |
|:---  |:--- |
| Спецификации температуры системы | <ul><li>Температура хранения: – 20 &deg; c – 50 &deg; c (– 4 &deg; f-122 &deg; f)</li><li>Непрерывная работа: 0 &deg; c – 40 &deg; c (32 &deg; F – 104 &deg; f)</li></ul> |
| Спецификации относительной влажности (RH) | <ul><li>Хранилище: от 5% до 95% относительной влажности</li><li>Относительная влажность при эксплуатации: 10% до 90%</li></ul>|
| Максимальные характеристики высоты | <ul><li>Эксплуатация: 15 000 футов (4 572 метров)</li><li>Без операционной системы: 40 000 футов (12 192 метров)</li></ul>|

> ![Значок уведомления ](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **Обратите внимание:** &nbsp; изменения или изменения, внесенные в оборудование, не утвержденные корпорацией Майкрософт, могут аннулировать права пользователя на его эксплуатацию.

Для КАНАДЫ и США:

Примечание. это оборудование было протестировано и найдено в соответствии с ограничениями для класса A Digital Device в соответствии с частью 15 правил FCC. Ограничения разработаны для того, чтобы обеспечить надежную защиту от негативного влияния при эксплуатации оборудования в коммерческой среде. Это оборудование генерирует, использует и излучает радиочастотную энергию. Поэтому ненадлежащая установка и использование могут привести к помехам в радиосвязи. Работа этого оборудования в жилых помещениях, вероятнее всего, может вызвать вредоносные помехи. в этом случае пользователю потребуется устранить помехи с учетом их собственных расходов.

Это оборудование соответствует требованиям части 15 правил ФКС, а также промышленным стандартам Канады в области оборудования радиопередачи, не требующего лицензирования. Эксплуатация допускается при следующих двух условиях: 1) устройство не создает вредных помех; 2) устройство принимает любые помехи, включая помехи, которые могут привести к нарушению работы устройства.

![Предупреждение о соответствии нормативным требованиям 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)

МОЖЕТ ICES-3 (A)/NMB-3 (A) корпорация Майкрософт, один Microsoft Way, Redmond, WA 98052, США.
США: (800) 426-9400 Канада: (800) 933-4750

Европейский союз: запросите копию Декларации ЕС.

> ![Значок предупреждения 13 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) . это класс продукта. В домашних условиях этот продукт может вызывать радиопомехи, и в этом случае пользователю потребуется принять адекватные меры.

Утилизация использованных элементов питания, электрического и электронного оборудования:

![Значок предупреждения 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Такой символ, указанный на самом устройстве, его элементе питания или упаковке, означает, что устройство и любые его элементы питания не должны утилизироваться вместе с бытовыми отходами. Пользователь обязан передать устройство и его элементы питания в уполномоченный пункт сбора для утилизации элементов питания, электрического и электронного оборудования. Раздельный сбор и утилизация отходов помогают экономить природные ресурсы и предотвращать негативное влияние на здоровье людей и окружающую среду из-за наличия опасных веществ в составе элементов питания, электрического и электронного оборудования, а также их неправильной утилизации. За дополнительными сведениями о местах сбора использованных элементов питания и электрического оборудования обратитесь в местные органы власти, местную службу утилизации бытовых отходов или магазин, где вы приобрели устройство. Свяжитесь с erecycle@microsoft.com, чтобы получить дополнительную информацию по утилизации отходов электрического и электронного оборудования.

Устройство содержит кнопочный элемент питания.
Microsoft Ирландия Сандифорд, средство EST Дублин D18 KX32 ИРЛ телефон номер: + 353 1 295 3826 номер факса: + 353 1 706 4110

## <a name="next-steps"></a>Дальнейшие действия

- [Подготовка к развертыванию Azure Stack пограничных Мини-R](azure-stack-edge-mini-r-deploy-prep.md)
