---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: 3312d1ec7c2535e103cf8959599c0d4c3014f520
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86218444"
---
1. [Подключитесь к интерфейсу PowerShell](#connect-to-the-powershell-interface).
2. Используйте `Get-HcsApplianceInfo` для получения сведений об устройстве.

    В следующем примере показано использование этого командлета:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Ниже приведена таблица с описанием некоторых важных сведений об устройстве.

    | Параметр | Описание |
    |-----------|-------------|
    | FriendlyName                   | Понятное имя устройства, настроенное с помощью локального веб-интерфейса во время развертывания устройства. В качестве понятного имени по умолчанию используется серийный номер устройства.  |
    | SerialNumber                   | Серийный номер устройства — это уникальный номер, назначенный фабрике.                                                                             |
    | Моделирование                          | Модель для Azure Stack пограничной или Шлюз Data Box устройства. Модель является физической для Azure Stack граничных и виртуальных Шлюз Data Box.                   |
    | FriendlySoftwareVersion        | Понятная строка, соответствующая версии программного обеспечения устройства. Для системы, в которой используется предварительная версия, понятное программное обеспечение будет Data Box Edge 1902. |
    | хксверсион                     | Версия программного обеспечения HCS, работающего на устройстве. Например, версия программного обеспечения HCS, соответствующая Data Box Edge 1902, — 1.4.771.324.            |
    | локалкапаЦитинмб              | Общая локальная емкость устройства в мегабит.                                                                                                        |
    | С регистрацией                   | Это значение указывает, активировано ли устройство в службе.                                                                                         |


