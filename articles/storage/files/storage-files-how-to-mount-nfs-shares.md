---
title: Подключение файлового ресурса Azure NFS к службе файлов Azure
description: Узнайте, как подключить общую сетевую папку файловой системы.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 530ae82720e6b4eb6a3e4d1021c0b37b9f4dbf5c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90707447"
---
# <a name="how-to-mount-an-nfs-file-share"></a>Как подключить файловый ресурс NFS

[Файлы Azure](storage-files-introduction.md) — это простая в использовании облачная файловая система от Майкрософт. Файловые ресурсы Azure можно подключить в дистрибутивах Linux с помощью протокола SMB или протокола сетевой файловой системы (NFS). Эта статья посвящена подключению к NFS. Дополнительные сведения о подключении с помощью SMB см. в статье [Использование службы файлов Azure с Linux](storage-how-to-use-files-linux.md). Дополнительные сведения о каждом из доступных протоколов см. в статье [протоколы файловых ресурсов Azure](storage-files-compare-protocols.md).

## <a name="limitations"></a>Ограничения

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Доступность по регионам

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Предварительные требования

- [Создайте общую папку NFS](storage-files-how-to-create-nfs-shares.md).

    > [!IMPORTANT]
    > Доступ к общим папкам NFS можно получить только из доверенных сетей. Подключения к общему ресурсу NFS должны исходить из одного из следующих источников:

- Используйте одно из следующих сетевых решений.
    - Либо [Создайте закрытую конечную точку](storage-files-networking-endpoints.md#create-a-private-endpoint) (рекомендуется), либо [Ограничьте доступ к общедоступной конечной точке](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Настройте VPN-подключение типа "точка — сеть" (P2S) в Linux для использования с файлами Azure](storage-files-configure-p2s-vpn-linux.md).
    - [Настройте VPN типа "сеть — сеть" для использования с файлами Azure](storage-files-configure-s2s-vpn.md).
    - Настройте [ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="disable-secure-transfer"></a>Отключить безопасную пересылку

1. Войдите в портал Azure и получите доступ к учетной записи хранения, содержащей созданный общий ресурс NFS.
1. Щелкните **Конфигурация**.
1. Для **обязательного безопасного перемещения**выберите **отключено** .
1. Щелкните **Сохранить**.

    :::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Снимок экрана с экраном настройки учетной записи хранения с отключенной безопасной защитой.":::

## <a name="mount-an-nfs-share"></a>Подключение общего ресурса NFS

1. После создания общей папки выберите общую папку и выберите **подключить из Linux**.
1. Введите путь подключения, который вы хотите использовать, а затем скопируйте сценарий.
1. Подключитесь к клиенту и используйте указанный сценарий подключения.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/mount-nfs-file-share-script.png" alt-text="Снимок экрана с экраном настройки учетной записи хранения с отключенной безопасной защитой.":::

Вы подключили общий ресурс NFS.

## <a name="next-steps"></a>Дальнейшие шаги

- Дополнительные сведения о службе файлов Azure см. в статье [Планирование развертывания файлов Azure](storage-files-planning.md).
- Если возникнут проблемы, см. раздел [Устранение неполадок файловых ресурсов NFS Azure](storage-troubleshooting-files-nfs.md).