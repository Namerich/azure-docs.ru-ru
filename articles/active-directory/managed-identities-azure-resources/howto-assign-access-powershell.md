---
title: Назначение управляемому удостоверению доступа к ресурсу с помощью PowerShell — Azure AD
description: Пошаговые инструкции по назначению управляемого удостоверения для одного ресурса, доступ к другому ресурсу с помощью PowerShell.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: b66567275bf2c7454a2d4bb87dcd4c14bb1fb9b4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90969286"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Назначение доступа на основе управляемого удостоверения к ресурсу с помощью PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

После настройки ресурса Azure с помощью управляемого удостоверения можно предоставить доступ на основе управляемого удостоверения другому ресурсу, как и любому субъекту безопасности. В этом примере показано, как предоставить управляемому удостоверению виртуальной машине Azure доступ к учетной записи хранения Azure с помощью PowerShell.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Предварительные требования

- Если вы не работали с управляемыми удостоверениями для ресурсов Azure, изучите [общие сведения](overview.md). **Обратите внимание на [различие между управляемыми удостоверениями, назначаемыми системой и назначаемыми пользователями](overview.md#managed-identity-types)**.
- Если у вас нет учетной записи Azure, [зарегистрируйтесь для получения бесплатной пробной учетной записи](https://azure.microsoft.com/free/), прежде чем продолжать.
- Выполнить примеры скриптов можно двумя способами:
    - используйте службу [Azure Cloud Shell](../../cloud-shell/overview.md), которую можно открыть с помощью кнопки **Попробовать** в правом верхнем углу блоков кода.
    - Выполните скрипты локально, установив последнюю версию [Azure PowerShell](/powershell/azure/install-az-ps), а затем войдите в Azure с помощью команды `Connect-AzAccount`. 

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Назначение управляемому удостоверению доступа к другому ресурсу с помощью Azure RBAC

1. Включите управляемое удостоверение в ресурсе Azure, [например на виртуальной машине Azure](qs-configure-powershell-windows-vm.md).

1. В этом примере мы предоставляем виртуальной машине Azure доступ к учетной записи хранения. Сначала с помощью [Get-AzVM](/powershell/module/az.compute/get-azvm) получите для виртуальной машины с именем `myVM` субъект-службу, который был создан при включении управляемого удостоверения. Затем с помощью [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) предоставьте этой виртуальной машине доступ для **чтения** к учетной записи хранения с именем `myStorageAcct`.

    ```azurepowershell-interactive
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Дальнейшие шаги

- [Что такое управляемые удостоверения для ресурсов Azure?](overview.md)
- Дополнительные сведения см. в статье [Настройка управляемых удостоверений для ресурсов Azure на виртуальной машине Azure с помощью PowerShell](qs-configure-powershell-windows-vm.md).
