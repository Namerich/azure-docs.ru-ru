---
title: Настройка туннеля VPN-пользователя Always-On
titleSuffix: Azure Virtual WAN
description: В этой статье описывается, как настроить туннель VPN пользователя Always On для виртуальной глобальной сети.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: e83ca64d2b0e50ec02007a3cd878e6bf034d0961
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91313592"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Настройка туннеля VPN-пользователя Always On для виртуальной глобальной сети

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Предварительные требования

Необходимо создать конфигурацию типа "точка — сеть" и изменить назначение виртуального концентратора. Инструкции см. в следующих разделах:

* [Создание конфигурации P2S](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Создание концентратора с помощью шлюза P2S](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-a-user-tunnel"></a>Настройка пользовательского туннеля

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Удаление профиля

Чтобы удалить профиль, выполните следующие действия.

1. Выполните следующую команду:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Отключите подключение и снимите флажок **подключиться автоматически** .

   ![Очистка](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о виртуальной глобальной сети см. в статье, содержащей [Часто задаваемые вопросы](virtual-wan-faq.md) о ней.
