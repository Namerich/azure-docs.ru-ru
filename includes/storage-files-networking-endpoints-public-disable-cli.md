---
title: включить файл
description: включить файл
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 195caa6f8d7c00c741741fbf80a77bd7fe5579b0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "84465048"
---
Следующая команда CLI блокирует весь трафик к общедоступной конечной точке учетной записи хранения. Обратите внимание, что в этой команде параметр `-bypass` имеет значение `AzureServices`. Это значение разрешает доступ к учетной записи хранения через общедоступную конечную точку для доверенных служб корпорации Майкрософт, таких как Синхронизация файлов Azure.

```bash
# This assumes $storageAccountResourceGroupName and $storageAccountName 
# are still defined from the beginning of this guide.
az storage account update \
    --resource-group $storageAccountResourceGroupName \
    --name $storageAccountName \
    --bypass "AzureServices" \
    --default-action "Deny" \
    --output none
```