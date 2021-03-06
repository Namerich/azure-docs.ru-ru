---
title: Сетевые топологии для миграций SQL Управляемый экземпляр
titleSuffix: Azure Database Migration Service
description: Изучите исходную и целевую конфигурации миграции Azure SQL Управляемый экземпляр с помощью Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: reference
ms.date: 01/08/2020
ms.openlocfilehash: ae036b7d893eb268ea55026054bf364dad0b610e
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94961555"
---
# <a name="network-topologies-for-azure-sql-managed-instance-migrations-using-azure-database-migration-service"></a>Сетевые топологии для миграции Управляемый экземпляр Azure SQL с помощью Azure Database Migration Service

В этой статье обсуждаются различные сетевые топологии, с которыми Azure Database Migration Service может работать для обеспечения полной миграции с серверов SQL Server на Управляемый экземпляр SQL Azure.

## <a name="azure-sql-managed-instance-configured-for-hybrid-workloads"></a>Управляемый экземпляр Azure SQL, настроенные для гибридных рабочих нагрузок 

Используйте эту топологию, если Управляемый экземпляр Azure SQL подключена к локальной сети. Такой подход упрощает сетевую маршрутизацию и максимально повышает пропускную способность во время миграции.

![Сетевая топология для гибридных рабочих нагрузок](media/resource-network-topologies/hybrid-workloads.png)

**Требования**

- В этом сценарии Управляемый экземпляр SQL и экземпляр Azure Database Migration Service создаются в одном виртуальная сеть Microsoft Azure, но они используют разные подсети.  
- Виртуальная сеть, используемая в этом сценарии, также подключается к локальной сети с помощью [ExpressRoute](../expressroute/expressroute-introduction.md) или [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

## <a name="sql-managed-instance-isolated-from-the-on-premises-network"></a>Управляемый экземпляр SQL изолированы от локальной сети

Используйте эту сетевую топологию, если в вашей среде нужно применить один или несколько из следующих сценариев:

- Управляемый экземпляр SQL изолированы от локальных подключений, но ваш экземпляр Azure Database Migration Service подключен к локальной сети.
- Если применяются политики управления доступом на основе ролей Azure (Azure RBAC) и необходимо ограничить доступ пользователей к той же подписке, где размещается Управляемый экземпляр SQL.
- Виртуальные сети, используемые для Управляемый экземпляр SQL и Azure Database Migration Service, находятся в разных подписках.

![Сетевая топология для Управляемого экземпляра Базы данных SQL Azure, изолированного от локальной сети](media/resource-network-topologies/mi-isolated-workload.png)

**Требования**

- Виртуальная сеть, которая Azure Database Migration Service используется для этого сценария, также должна быть подключена к локальной сети с помощью ( https://docs.microsoft.com/azure/expressroute/expressroute-introduction) или [VPN-подключения](../vpn-gateway/vpn-gateway-about-vpngateways.md)).
- Настройте [пиринг сети](../virtual-network/virtual-network-peering-overview.md) виртуальной сети между виртуальной сетью, используемой для SQL Управляемый экземпляр и Azure Database Migration Service.

## <a name="cloud-to-cloud-migrations-shared-virtual-network"></a>Миграция из облака в облако: общая виртуальная сеть

Используйте эту топологию, если исходный SQL Server размещается на виртуальной машине Azure и использует ту же виртуальную сеть с SQL Управляемый экземпляр и Azure Database Migration Service.

![Топология сети для миграции из облака в облако с помощью общей виртуальной сети](media/resource-network-topologies/cloud-to-cloud.png)

**Требования**

- Дополнительных требований нет.

## <a name="cloud-to-cloud-migrations-isolated-virtual-network"></a>Миграция облака в облако: изолированная виртуальная сеть

Используйте эту сетевую топологию, если в вашей среде нужно применить один или несколько из следующих сценариев:

- Управляемый экземпляр SQL подготавливается в изолированной виртуальной сети.
- Если применяются политики управления доступом на основе ролей Azure (Azure RBAC) и необходимо ограничить доступ пользователей к той же подписке, где размещается Управляемый экземпляр SQL.
- Виртуальные сети, используемые для SQL Управляемый экземпляр и Azure Database Migration Service, находятся в разных подписках.

![Топология сети для миграции из облака в облако с помощью изолированной виртуальной сети](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Требования**

- Настройте [пиринг сети](../virtual-network/virtual-network-peering-overview.md) виртуальной сети между виртуальной сетью, используемой для SQL Управляемый экземпляр и Azure Database Migration Service.

## <a name="inbound-security-rules"></a>Правила безопасности для входящего трафика

| **ИМЯ**   | **ПОРТУ** | **СМ** | **ИСТОЧНИКА** | **МЕСТОНАЗНАЧЕНИЕ** | **ДЕЙСТВИЕ** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Любой      | Любой          | Подсеть DMS | Любой             | Allow      |

## <a name="outbound-security-rules"></a>Правила безопасности для исходящего трафика

| **ИМЯ**                  | **ПОРТУ**                                              | **СМ** | **ИСТОЧНИКА** | **МЕСТОНАЗНАЧЕНИЕ**           | **ДЕЙСТВИЕ** | **Причина для правила**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| управление                | 443,9354                                              | TCP          | Любой        | Любой                       | Allow      | Взаимодействие плоскости управления через служебную шину и хранилище BLOB-объектов Azure. <br/>(Если включен пиринг Майкрософт, это правило может не понадобиться.)                                                             |
| Диагностика               | 12000                                                 | TCP          | Любой        | Любой                       | Allow      | DMS использует это правило для сбора диагностических сведений для устранения неполадок.                                                                                                                      |
| Исходный SQL Server         | 1433 (или порт TCP IP, который прослушивает SQL Server) | TCP          | Любой        | локальное адресное пространство; | Allow      | Возможность подключения к исходному экземпляру SQL Server из DMS. <br/>(Если есть подключение типа "сайт-сайт", это правило может не понадобиться.)                                                                                       |
| Именованный экземпляр SQL Server | 1434                                                  | UDP          | Любой        | локальное адресное пространство; | Allow      | Возможность подключения к исходному именованному экземпляру SQL Server из DMS. <br/>(Если есть подключение типа "сайт-сайт", это правило может не понадобиться.)                                                                        |
| Общий доступ по SMB                 | 445                                                   | TCP          | Любой        | локальное адресное пространство; | Allow      | Общий сетевой доступ по SMB в DMS для хранения файлов резервных копий базы данных с целью переноса в управляемый экземпляр Базы данных SQL Azure и SQL Server на виртуальной машине Azure. <br/>(Если есть подключение типа "сайт-сайт", это правило может не понадобиться.) |
| DMS_subnet                | Любой                                                   | Любой          | Любой        | DMS_subnet                | Allow      |                                                                                                                                                                                                  |

## <a name="see-also"></a>См. также

- [Миграция SQL Server в SQL Управляемый экземпляр](./tutorial-sql-server-to-managed-instance.md)
- [Общие сведения о предварительных требованиях для использования Azure Database Migration Service](./pre-reqs.md)
- [Создание виртуальной сети с помощью портала Azure](../virtual-network/quick-create-portal.md)

## <a name="next-steps"></a>Следующие шаги

- Общие сведения о Azure Database Migration Service см. в статье [что такое Azure Database Migration Service?](dms-overview.md).
- Текущие сведения о региональных доступности Azure Database Migration Service см. на странице доступность [продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) .