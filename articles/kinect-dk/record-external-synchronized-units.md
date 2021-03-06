---
title: Использование средства записи Azure Kinect с внешними синхронизированными устройствами
description: Узнайте, как записывать данные с устройств, настроенных для внешней синхронизации с помощью средства записи Azure Kinect.
author: tesych
ms.author: tesych
ms.reviewer: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, датчик, средство просмотра, внешняя синхронизация, задержка этапа, глубина, RGB, Камера, звуковой кабель, устройство записи
ms.openlocfilehash: 052f6f1ac9f90e764de25d1d4d1b25b3d50a848d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276917"
---
# <a name="use-azure-kinect-recorder-with-external-synchronized-devices"></a>Использование средства записи Azure Kinect с внешними синхронизированными устройствами

В этой статье содержатся сведения о том, как средство [записи Azure Kinect](azure-kinect-recorder.md) может записывать данные, настроенные на внешних устройствах синхронизации.

## <a name="prerequisites"></a>Предварительные требования

- [Настройте несколько единиц Azure KINECT DK для внешней синхронизации](https://support.microsoft.com/help/4494429).

## <a name="external-synchronization-constraints"></a>Внешние ограничения синхронизации

- НА главном устройстве не может быть СИНХРОНИЗИРОВАНо подключение кабеля.
- Для включения синхронизации основное устройство должно выполнять потоковую передачу камеры RGB.
- Все единицы должны использовать одну и ту же конфигурацию камеры (частота кадров и разрешение).
- Все единицы должны запускать одно встроенное по устройства (см. инструкции по[обновлению встроенного по](update-device-firmware.md) ).
- Все подчиненные устройства должны быть запущены перед главным устройством.
- Одно и то же значение экспозиции должно быть установлено на всех устройствах.
- Каждый подчиненный параметр *откладывается* относительно главного устройства.

## <a name="record-when-each-unit-has-a-host-pc"></a>Запись, когда у каждой единицы есть главный компьютер

В приведенном ниже примере каждое устройство имеет собственный выделенный главный компьютер.
Рекомендуется подключать устройства к выделенным компьютерам, чтобы избежать проблем с пропускной способностью USB и использованием ЦП и GPU.

### <a name="subordinate-1"></a>Подчиненный-1

1. Настройка средства записи для первой единицы измерения

      `k4arecorder.exe --external-sync sub -e -8 -r 5 -l 10 sub1.mkv`

2. Устройство начинает ожидание

    ```console
    Device serial number: 000011590212
    Device version: Rel; C: 1.5.78; D: 1.5.60[6109.6109]; A: 1.5.13
    Device started
    [subordinate mode] Waiting for signal from master
    ```

### <a name="subordinate-2"></a>Подчиненный — 2

1. Настройка средства записи для второй единицы измерения

    `k4arecorder.exe --external-sync sub -e -8 -r 5 -l 10 sub2.mkv`

2. Устройство начинает ожидание

    ```console
    Device serial number: 000011590212
    Device version: Rel; C: 1.5.78; D: 1.5.60[6109.6109]; A: 1.5.13
    Device started
    [subordinate mode] Waiting for signal from master
    ```

### <a name="master"></a>master.

1. Начать запись в мастере

    `>k4arecorder.exe --external-sync master -e -8 -r 5 -l 10 master.mkv`

2. Дождитесь завершения записи

## <a name="recording-when-multiple-units-connected-to-single-host-pc"></a>Запись при подключении нескольких единиц к компьютеру с одним узлом

Можно использовать несколько ДКС Azure Kinect, подключенных к одному главному компьютеру. Однако это может быть очень требовательно к пропускной способности USB и вычислению на узле. Чтобы уменьшить потребность, сделайте следующее:

- Подключите каждое устройство к собственному USB-контроллеру узла.
- Иметь мощный графический процессор, который может поддерживать механизм глубины для каждого устройства.
- Записывайте только необходимые датчики и используйте более низкие частоты кадров.

Всегда запускайте подчиненные устройства первыми и последними.

## <a name="subordinate-1"></a>Подчиненный-1

1. Запустить средство записи на подчиненном

    `>k4arecorder.exe --device 1 --external-sync subordinate --imu OFF -e -8 -r 5 -l 5 output-2.mkv`

2. Устройство переходит в состояние ожидания

## <a name="master"></a>master.

1. Запустить главное устройство

    `>k4arecorder.exe --device 0 --external-sync master --imu OFF -e -8 -r 5 -l 5 output-1.mkv`

2. Ожидание завершения записи

## <a name="playing-recording"></a>Воспроизведение записи

Для воспроизведения записи можно использовать [средство просмотра Kinect Azure](azure-kinect-viewer.md) .



## <a name="tips"></a>Советы

- Использование раскрытия вручную для записи синхронизированных камер. Автоматическая экспозиция камеры RGB может повлиять на синхронизацию времени.
- Перезапуск подчиненного устройства приведет к потере синхронизации.
- Некоторые [режимы камеры](hardware-specification.md#depth-camera-supported-operating-modes) поддерживают 15 ГБ/с. Мы рекомендуем не смешивать режимы и частоту кадров между устройствами
- Подключение нескольких устройств к одному компьютеру может легко регулировать пропускную способность USB, рассмотрите возможность использования отдельного компьютера для каждого устройства. Обратите особое внимание на вычислительные ресурсы ЦП и GPU.
- Отключите микрофон и иму, если они не нужны для повышения надежности.

Сведения о проблемах см. в [статье Устранение неполадок](troubleshooting.md)

## <a name="see-also"></a>См. также

- [Настройка внешней синхронизации](https://support.microsoft.com/help/4494429/sync-multiple-devices)
- [Средство записи Azure Kinect](azure-kinect-recorder.md) для настройки средства записи и дополнительные сведения.
- [Средство просмотра Kinect Azure](azure-kinect-viewer.md) для воспроизведения записей или задания свойств камеры RGB, недоступных через средство записи.
- [Средство встроенного по Azure Kinect](azure-kinect-firmware-tool.md) для обновления встроенного по устройства.
