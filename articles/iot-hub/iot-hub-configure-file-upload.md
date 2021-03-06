---
title: Настройка отправки файлов с помощью портала Azure | Документация Майкрософт
description: Использование портала Azure для настройки Центра Интернета вещей, чтобы включить отправку файлов с подключенных устройств. Содержит сведения о настройке целевой учетной записи хранения Azure.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robinsh
ms.openlocfilehash: da28bfa31c74ff33a200967267500033dd6a9b1b
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535881"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Настройка отправки файлов Центра Интернета вещей с помощью портала Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Передача файла

Чтобы использовать [функции передачи файлов в Центре Интернета вещей](iot-hub-devguide-file-upload.md), сначала необходимо связать учетную запись хранения Azure с центром. Выберите **Отправка файла** , чтобы отобразить список свойств передачи файлов для Центра Интернета вещей, который вы изменяете.

![Просмотр параметров отправки файлов в Центре Интернета вещей на портале](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Контейнер хранилища.** На портале Azure выберите контейнер больших двоичных объектов в учетной записи хранения своей текущей подписки Azure, чтобы связать его с Центром Интернета вещей. При необходимости можно создать новую учетную запись хранения Azure в колонке **Учетные записи хранения** и новый контейнер больших двоичных объектов в колонке **Контейнеры** . Центр Интернета вещей автоматически генерирует универсальные коды ресурсов (URI) подписанных URL-адресов с разрешениями на запись в этом контейнере больших двоичных объектов, чтобы устройства могли их использовать во время передач файлов.

   ![Просмотр контейнеров хранилища для отправки файла на портале](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Receive notifications for uploaded files** (Получать уведомления об отправленных файлах). Включите или отключите уведомления об отправке файлов с помощью переключателя.

* **SAS TTL** (Срок жизни SAS). Этот параметр определяет срок жизни универсальных кодов ресурса (URI) SAS, возвращаемых Центром Интернета вещей на устройство. По умолчанию устанавливается значение "один час", но его можно изменить с помощью ползунка.

* **File notification settings default TTL** (Стандартный срок жизни уведомления о файле). Срок жизни уведомления об отправке файла. По умолчанию устанавливается значение "один день", но его можно изменить с помощью ползунка.

* **File notification maximum delivery count** (Максимальное число доставок уведомления о файле): число попыток доставки уведомления о передаче файла, предпринимаемых Центром Интернета вещей. По умолчанию устанавливается значение 10, но его можно изменить с помощью ползунка.

   ![Настройка отправки файла Центра Интернета вещей на портале](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о возможностях Центра Интернета вещей, касающихся отправки файлов, см. в разделе [Отправка файлов с помощью Центра Интернета вещей](iot-hub-devguide-file-upload.md) руководства для разработчиков по Центру Интернета вещей.

Дополнительные сведения об управлении Центром Интернета вещей в Azure см. по следующим ссылкам:

* [Массовое управление удостоверениями устройств Центра Интернета вещей](iot-hub-bulk-identity-mgmt.md)
* [Мониторинг центра Интернета вещей](monitor-iot-hub.md)

Для дальнейшего изучения возможностей Центра Интернета вещей см. следующие статьи:

* [Руководство разработчика для Центра Интернета вещей](iot-hub-devguide.md)
* [Краткое руководство. Развертывание первого модуля IoT Edge на устройстве под управлением 64-разрядной ОС Linux](../iot-edge/quickstart-linux.md)
* [Все аспекты безопасности решения Центра Интернета вещей](../iot-fundamentals/iot-security-ground-up.md)