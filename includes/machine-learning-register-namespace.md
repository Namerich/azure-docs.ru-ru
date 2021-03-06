---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: 6e9f349f2fd90fdb12d8d26ca904e504b75d9db2
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96445199"
---
* При создании рабочей области можно либо автоматически создать службы, необходимые для рабочей области, либо использовать существующие службы. Если вы хотите использовать __существующие службы из другой подписки Azure__ по сравнению с рабочей областью, необходимо зарегистрировать машинное обучение Azureое пространство имен в подписке, которая содержит эти службы. Например, при создании рабочей области в подписке A, которая использует учетную запись хранения из подписки B, пространство имен Машинное обучение Azure должно быть зарегистрировано в подписке B, прежде чем можно будет использовать учетную запись хранения с рабочей областью.

    Поставщик ресурсов для Машинное обучение Azure — это __Microsoft. мачинелеарнингсервице__. Сведения о том, как узнать, зарегистрирован ли он и как его зарегистрировать, см. в статье [поставщики и типы ресурсов Azure](../articles/azure-resource-manager/management/resource-providers-and-types.md)  .

    > [!IMPORTANT]
    > Это относится только к ресурсам, предоставленным при создании рабочей области; Учетные записи хранения Azure, регистрация контейнеров Azure, Azure Key Vault и Application Insights.