---
title: Перенос виртуальных машин Azure на управляемые диски
description: Миграция виртуальных машин Azure, созданных с помощью неуправляемых дисков в учетных записях хранения, на Управляемые диски.
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d88792f50e0e79dd0313694cf979761054551eac
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487530"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Миграция виртуальных машин Azure на Управляемые диски Azure

Управляемые диски Azure упрощают управление хранилищем, избавляя от необходимости отдельно управлять учетными записями хранения.  Можно также перенести имеющиеся виртуальные машины Azure на Управляемые диски, чтобы использовать преимущества надежности виртуальных машин в группе доступности. Это гарантирует, что диски различных виртуальных машин в группе доступности будут достаточно изолированы друг от друга, чтобы избежать единой точки отказа. В этом случае диски разных виртуальных машин автоматически помещаются в группу доступности в разные единицы масштабирования (метки) хранилища, что позволяет снизить влияние сбоя, происходящего в отдельной единице масштабирования хранилища из-за проблем с оборудованием или программным обеспечением.
В зависимости от потребностей вы можете выбрать один из четырех уровней хранилища. Дополнительные сведения о доступных типах дисков см. в статье [Выбор типа диска](../disks-types.md) .

## <a name="migration-scenarios"></a>Сценарии миграции

Ниже приведены сценарии перехода на Управляемые диски.

|Сценарий  |Статья  |
|---------|---------|
|Преобразование автономных виртуальных машин и виртуальных машин в набор доступности для управляемых дисков     |[Переключение виртуальной машины с неуправляемых на управляемые диски](convert-unmanaged-to-managed-disks.md)         |
|Преобразование одной виртуальной машины из классической в диспетчер ресурсов на управляемых дисках     |[Создание виртуальной машины на классическом виртуальном жестком диске](create-vm-specialized-portal.md)         |
|Преобразование всех виртуальных машин в виртуальной сети из классической модели в диспетчер ресурсов на управляемых дисках     |[Перенос ресурсов IaaS из классической модели в модель Azure Resource Manager с помощью Azure PowerShell](../migration-classic-resource-manager-ps.md), затем [Convert a VM from unmanaged disks to managed disks](convert-unmanaged-to-managed-disks.md) (Преобразование виртуальной машины с неуправляемыми дисками для использования управляемых дисков)         |
|Обновление виртуальных машин с использованием стандартных неуправляемых дисков на виртуальных машинах с управляемыми дисками уровня "Премиум"     | Сначала [преобразуйте виртуальную машину Windows с неуправляемыми дисками на управляемые диски](convert-unmanaged-to-managed-disks.md). Затем [Обновите тип хранилища управляемого диска](convert-disk-storage.md).         |

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об [управляемых дисках](../managed-disks-overview.md)
- Ознакомьтесь с [ценами на Управляемые диски](https://azure.microsoft.com/pricing/details/managed-disks/).