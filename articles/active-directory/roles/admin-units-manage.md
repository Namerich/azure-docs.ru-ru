---
title: Добавление и удаление административных единиц — Azure Active Directory | Документация Майкрософт
description: Используйте административные единицы, чтобы ограничить область разрешений роли в Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: roles
ms.workload: identity
ms.date: 11/04/2020
ms.author: curtand
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: bbe76d21d948525421790f574830da7b3a163216
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93395399"
---
# <a name="manage-administrative-units-in-azure-active-directory"></a>Управление административными единицами в Azure Active Directory

Для более детального административного управления в Azure Active Directory (Azure AD) можно назначить пользователей роли Azure AD с областью, ограниченной одним или несколькими административными единицами.

## <a name="get-started"></a>Начало работы

1. Чтобы выполнить запросы из следующих инструкций с помощью [Graph Explorer](https://aka.ms/ge), выполните следующие действия.

    a. На портале Azure перейдите к Azure AD. 
    
    b. В списке приложений выберите **Graph Explorer**.
    
    c. На панели **разрешения** выберите **предоставить согласие администратора для Graph Explorer**.

    ![Снимок экрана, показывающий ссылку "предоставление согласия администратора для Graph Explorer".](./media/admin-units-manage/select-graph-explorer.png)


1. Используйте предварительную версию Azure AD PowerShell.

## <a name="add-an-administrative-unit"></a>Добавление административной единицы

Административную единицу можно добавить с помощью портал Azure или PowerShell.

### <a name="use-the-azure-portal"></a>Использование портала Azure

1. На портале Azure перейдите к Azure AD. Затем на левой панели выберите **административные единицы**.

    ![Снимок экрана: ссылка "административные единицы" в Azure AD.](./media/admin-units-manage/nav-to-admin-units.png)

1. Нажмите кнопку **Добавить** в верхней части панели, а затем в поле **имя** введите имя административной единицы. При необходимости добавьте описание административной единицы.

    ![Снимок экрана, показывающий кнопку "Добавить", и поле "имя" для ввода имени административной единицы.](./media/admin-units-manage/add-new-admin-unit.png)

1. Нажмите синюю кнопку **Добавить** , чтобы завершить административную единицу.

### <a name="use-powershell"></a>Использование PowerShell

Прежде чем пытаться выполнить следующие команды, установите Azure AD PowerShell (Предварительная версия):

```powershell
Connect-AzureAD
New-AzureADMSAdministrativeUnit -Description "West Coast region" -DisplayName "West Coast"
```

При необходимости можно изменить значения, заключенные в кавычки.

### <a name="use-microsoft-graph"></a>Использование Microsoft Graph

```http
Http Request
POST /administrativeUnits
Request body
{
  "displayName": "North America Operations",
  "description": "North America Operations administration"
}
```

## <a name="remove-an-administrative-unit"></a>Удаление административной единицы

В Azure AD вы можете удалить административную единицу, которая больше не нужна в качестве единицы области для административных ролей.

### <a name="use-the-azure-portal"></a>Использование портала Azure

1. В портал Azure перейдите в **Azure AD** и выберите **административные единицы**. 
1. Выберите удаляемую административную единицу и нажмите кнопку **Удалить**. 
1. Чтобы подтвердить удаление административной единицы, выберите **Да**. Административная единица удаляется.

![Снимок экрана: кнопка удаления административной единицы и окно подтверждения.](./media/admin-units-manage/select-admin-unit-to-delete.png)

### <a name="use-powershell"></a>Использование PowerShell

```powershell
$delau = Get-AzureADMSAdministrativeUnit -Filter "displayname eq 'DeleteMe Admin Unit'"
Remove-AzureADMSAdministrativeUnit -ObjectId $delau.ObjectId
```

Можно изменить значения, заключенные в кавычки, в соответствии с требованиями конкретной среды.

### <a name="use-the-graph-api"></a>Использование API Graph

```http
HTTP request
DELETE /administrativeUnits/{Admin id}
Request body
{}
```

## <a name="next-steps"></a>Дальнейшие шаги

* [Управление пользователями в административной единице](admin-units-add-manage-users.md)
* [Управление группами в административной единице](admin-units-add-manage-groups.md)
