---
title: Решение Azure VMware — DNS-пересылка из частного облака в локальную среду
description: В этой статье описано, как включить DNS-сервер частного облака Клаудсимпле для прямого поиска локальных ресурсов.
author: sharaths-cs
ms.author: b-shsury
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3571455db6ecc600bf0948087b40c281d72512ce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87091255"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>Включение DNS-серверов частного облака Клаудсимпле для пересылки DNS-запросов локальных ресурсов на DNS-серверы

DNS-серверы частного облака могут пересылать DNS для любых локальных ресурсов на DNS-серверы.  Включение поиска позволяет компонентам частного облака vSphere искать любые службы, работающие в локальной среде, и взаимодействовать с ними с помощью полных доменных имен (FQDN).

## <a name="scenarios"></a>Сценарии 

Пересылка поиска DNS для локального DNS-сервера позволяет использовать частное облако в следующих сценариях:

* Использование частного облака в качестве настройки аварийного восстановления для локального решения VMware
* Использование локального Active Directory в качестве источника удостоверений для частного облака vSphere
* Использование ХККС для миграции виртуальных машин из локальной среды в частное облако

## <a name="before-you-begin"></a>Перед началом

Сетевое подключение должно присутствовать в сети частного облака к локальной сети для работы пересылки DNS.  Можно настроить сетевое подключение с помощью:

* [Подключение из локальной среды к Клаудсимпле с помощью ExpressRoute](on-premises-connection.md)
* [Настройка VPN-шлюза типа "сеть — сеть"](./vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)

Для этого подключения необходимо открыть порты брандмауэра, чтобы пересылка DNS работала.  Используются порты TCP 53 или UDP-порт 53.

> [!NOTE]
> Если вы используете VPN типа "сеть — сеть", подсеть локального DNS-сервера должна быть добавлена как часть локальных префиксов.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Запрос пересылки DNS из частного облака в локальную среду

Чтобы включить DNS-пересылку из частного облака в локальную среду, отправьте [запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), предоставив следующие сведения.

* Тип проблемы: **Техническая**
* Подписка: **Подписка, в которой развернута служба клаудсимпле**
* Служба: **решение VMware по клаудсимпле**
* Тип проблемы: **рекомендации или разделы справки...**
* Подтип проблемы: **нужна помощь с NW**
* Укажите доменное имя локального домена в области сведений.
* Укажите список локальных DNS-серверов, на которые будет перенаправлен Поиск из частного облака в области сведений.

## <a name="next-steps"></a>Дальнейшие действия

* [Дополнительные сведения о конфигурации локального брандмауэра](on-premises-firewall-configuration.md)
* [Конфигурация локального DNS-сервера](on-premises-dns-setup.md)
