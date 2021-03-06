---
title: Выявление отключенных дисков Azure — портал Azure
description: Как найти отключенные управляемые и неуправляемые диски Azure (виртуальные жесткие диски и страничные BLOB-объекты) с помощью портала Azure.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 06/01/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 6632d65fa07788e35b24c2f957e713f824f6b091
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87542744"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks---azure-portal"></a>Поиск и удаление отключенных управляемых и неуправляемых дисков Azure — портал Azure

Когда вы удаляете виртуальную машину в Azure, подключенные к ней диски не удаляются по умолчанию. Это предотвращает потерю данных из-за случайного удаления виртуальной машины. Тем не менее вы продолжаете платить за неподключенные диски. В этой статье показано, как найти и удалить все отключенные диски с помощью портала Azure, чтобы сократить затраты.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Управляемые диски. Поиск и удаление неподключенных дисков

Если у вас есть отключенные управляемые диски и данные на них больше не нужны, воспользуйтесь описанной ниже процедурой, чтобы найти их на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).
1. Найдите в поиске и выберите пункт **Диски**.

    В области **Диски** отображается список всех дисков. Все диски с пометкой **-** в столбце **Владелец** являются отключенными.

    [![Снимок экрана: колонка "управляемые диски", если диск содержит столбец "владелец", это неприсоединенный диск.](media/disks-find-unattached-portal/managed-disk-unattached-owner.png)](media/disks-find-unattached-portal/managed-disk-owner-unattached.png#lightbox)

1. Выберите отключенный диск, который нужно удалить. Откроется область этого диска.
1. В области диска можно еще раз проверить его состояние (отключен), а затем нажать **Удалить**.

    :::image type="content" source="media/disks-find-unattached-portal/delete-managed-disk-unattached.png" alt-text="Снимок экрана: область отдельного управляемого диска. Если диск отключен, в этой области отображается состояние Отключен. Вы можете удалить этот диск, если данные на нем больше не нужны.":::

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Неуправляемые диски. Поиск и удаление неподключенных дисков

Неуправляемые диски — это VHD-файлы, которые хранятся в [учетных записях хранения Azure](../storage/common/storage-account-overview.md) как [страничные BLOB-объекты](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs).

Если у вас есть неуправляемые диски, которые не подключены к виртуальной машине и не содержат нужных данных, и вы хотите их удалить, воспользуйтесь описанной ниже процедурой, чтобы найти их на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com/).
1. Найдите в поиске и выберите пункт **Диски (классические)** .

    Появится список всех неуправляемых дисков. Все диски с пометкой **-** в столбце **Присоединено к** являются отключенными.

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-attached-to.png" alt-text="Снимок экрана: область отдельного управляемого диска. Если диск отключен, в этой области отображается состояние Отключен. Вы можете удалить этот диск, если данные на нем больше не нужны.":::

1. Выберите отключенный диск, который нужно удалить. Откроется область этого диска.

1. В области диска можно еще раз удостовериться, что он отключен, так как столбец **Присоединено к** будет по-прежнему содержать **-** .

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-select-blade.png" alt-text="Снимок экрана: область отдельного управляемого диска. Если диск отключен, в этой области отображается состояние Отключен. Вы можете удалить этот диск, если данные на нем больше не нужны.":::

1. Выберите команду **Удалить**.

    :::image type="content" source="media/disks-find-unattached-portal/delete-unmanaged-disk-unattached.png" alt-text="Снимок экрана: область отдельного управляемого диска. Если диск отключен, в этой области отображается состояние Отключен. Вы можете удалить этот диск, если данные на нем больше не нужны.":::

## <a name="next-steps"></a>Дальнейшие действия

Если вы хотите автоматизировать поиск и удаление неподключенных учетных записей хранения, ознакомьтесь со статьями о [CLI](linux/find-unattached-disks.md) и [PowerShell](windows/find-unattached-disks.md).

Дополнительные сведения см. в статье об [удалении учетной записи хранения](../storage/common/storage-account-create.md#delete-a-storage-account) и в записи блога о [выявлении потерянных дисков с помощью PowerShell](/archive/blogs/ukplatforms/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell).
