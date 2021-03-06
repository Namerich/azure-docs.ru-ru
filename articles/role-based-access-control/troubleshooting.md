---
title: Устранение неполадок в Azure RBAC
description: Устранение неполадок в управлении доступом на основе ролей в Azure (Azure RBAC).
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1, devx-track-azurecli
ms.openlocfilehash: e30af9522d7c8fa81c4d93e11d252aefc4426586
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96184269"
---
# <a name="troubleshoot-azure-rbac"></a>Устранение неполадок в Azure RBAC

В этой статье содержатся ответы на некоторые распространенные вопросы об управлении доступом на основе ролей в Azure (Azure RBAC), чтобы вы узнали, что следует рассчитывать при использовании ролей и может устранять неполадки доступа.

## <a name="azure-role-assignments-limit"></a>Ограничение назначений ролей Azure

Azure поддерживает до **2000** назначений ролей на подписку. Это ограничение распространяется на назначения ролей в пределах подписки, группы ресурсов и ресурсов. Если вы получаете сообщение об ошибке "больше не удается создать назначения ролей (код: Ролеассигнментлимитексцеедед)" при попытке назначить роль, попробуйте уменьшить число назначений ролей в подписке.

> [!NOTE]
> Ограничение на количество назначений ролей **2000** для каждой подписки фиксировано и не может быть увеличено.

Если вы близки к этому ограничению, вот несколько способов уменьшить число назначений ролей:

- Добавьте пользователей в группы и назначьте роли группам. 
- Объедините несколько встроенных ролей с пользовательской ролью. 
- Сделайте распространенные назначения ролей в более высокой области, например в подписке или группе управления.
- Если у вас Azure AD Premium P2, сделайте назначения ролей доступными в [Azure AD privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md) вместо постоянного назначения. 
- Добавьте дополнительную подписку. 

Чтобы узнать число назначений ролей, можно просмотреть [диаграмму на странице управления доступом (IAM)](role-assignments-list-portal.md#list-number-of-role-assignments) в портал Azure. Можно также использовать следующие Azure PowerShell команды:

```azurepowershell
$scope = "/subscriptions/<subscriptionId>"
$ras = Get-AzRoleAssignment -Scope $scope | Where-Object {$_.scope.StartsWith($scope)}
$ras.Count
```

## <a name="problems-with-azure-role-assignments"></a>Проблемы с назначениями ролей Azure

- Если не удается добавить назначение роли в портал Azure **управления доступом (IAM)** , так как параметр **Добавить**  >  **назначение роли** отключен или получено сообщение об ошибке "клиент с идентификатором объекта не имеет разрешения на выполнение действия", убедитесь, что вы вошли в систему с помощью пользователя, которому назначена роль с `Microsoft.Authorization/roleAssignments/write` таким разрешением, как [владелец](built-in-roles.md#owner) или [администратор доступа пользователей](built-in-roles.md#user-access-administrator) в области, которой вы пытаетесь назначить роль.
- Если для назначения ролей используется субъект-служба, может возникнуть ошибка "недостаточно прав для выполнения операции". Например, предположим, что у вас есть субъект-служба, которому назначена роль владельца, и вы пытаетесь создать следующее назначение роли в качестве субъекта-службы с помощью Azure CLI:

    ```azurecli
    az login --service-principal --username "SPNid" --password "password" --tenant "tenantid"
    az role assignment create --assignee "userupn" --role "Contributor"  --scope "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}"
    ```

    Если появляется сообщение об ошибке "недостаточно прав для выполнения операции", вероятно, Azure CLI пытается найти удостоверение уполномоченного пользователя в Azure AD, а субъект-служба не может прочитать Azure AD по умолчанию.

    Существует два способа устранения этой ошибки. Первый способ — назначить роль " [читатели каталога](../active-directory/roles/permissions-reference.md#directory-readers) " субъекту-службе, чтобы она могла считывать данные в каталоге.

    Второй способ устранения этой ошибки — создание назначения роли с помощью `--assignee-object-id` параметра, а не `--assignee` . С помощью `--assignee-object-id` Azure CLI пропустит Поиск Azure AD. Необходимо получить идентификатор объекта пользователя, группы или приложения, которым требуется назначить роль. Дополнительные сведения см. в статье [Добавление и удаление назначений ролей Azure с помощью Azure CLI](role-assignments-cli.md#add-role-assignment-for-a-new-service-principal-at-a-resource-group-scope).

    ```azurecli
    az role assignment create --assignee-object-id 11111111-1111-1111-1111-111111111111  --role "Contributor" --scope "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}"
    ```
- Если вы попытаетесь удалить последнее назначение роли владельца для подписки, может появиться сообщение об ошибке "не удается удалить последнее назначение администратора RBAC. Удаление назначения роли последнего владельца для подписки не поддерживается, чтобы избежать потери подписки. Если вы хотите отменить подписку, см. статью [Отмена подписки Azure](../cost-management-billing/manage/cancel-azure-subscription.md).

## <a name="problems-with-custom-roles"></a>Проблемы с пользовательскими ролями

- Инструкции по созданию настраиваемой роли см. в руководствах по настраиваемым ролям с помощью [портал Azure](custom-roles-portal.md) (в настоящее время в предварительной версии), [Azure PowerShell](tutorial-custom-role-powershell.md)или [Azure CLI](tutorial-custom-role-cli.md).
- Если вы не можете обновить существующую пользовательскую роль, убедитесь, что вы вошли в систему с помощью пользователя, которому назначена роль с `Microsoft.Authorization/roleDefinition/write` разрешением, таким как [владелец](built-in-roles.md#owner) или [администратор доступа пользователей](built-in-roles.md#user-access-administrator).
- Если не удается удалить пользовательскую роль и отображается сообщение об ошибке "Существуют назначения ролей, которые ссылаются на роль (код: RoleDefinitionHasAssignments)", это означает, что все еще есть назначения этой пользовательской роли. Удалите эти назначения и снова попытайтесь удалить пользовательскую роль.
- Если при попытке создать пользовательскую роль отображается сообщение об ошибке "Превышено ограничение на определение ролей. Невозможно создать дополнительные определения ролей (код: Роледефинитионлимитексцеедед). при попытке создать новую пользовательскую роль удалите все пользовательские роли, которые не используются. Azure поддерживает до **5000** пользовательских ролей в каталоге. (Для Azure для Германии и Azure для Китая с помощью 21Vianet ограничение составляет 2000 пользовательских ролей.)
- Если появляется сообщение об ошибке, похожее на "клиент имеет разрешение на выполнение действия" Microsoft. Authorization/roleDefinitions/Write "в области"/субскриптионс/{субскриптионид} ", но связанная подписка не найдена при попытке обновить пользовательскую роль, проверьте, не была ли удалена одна или несколько [назначаемых областей](role-definitions.md#assignablescopes) в каталоге. Если область удалена, создайте запрос в службу поддержки, так как способа самостоятельного решения этой проблемы пока не существует.

## <a name="custom-roles-and-management-groups"></a>Пользовательские роли и группы управления

- В пользовательской роли можно определить только одну группу управления `AssignableScopes` . В настоящее время добавление группы управления в `AssignableScopes` доступно в режиме предварительной версии.
- Пользовательские роли с `DataActions` не могут быть назначены в области группы управления.
- Azure Resource Manager не проверяет существование группы управления в назначаемой области определения роли.
- Дополнительные сведения о настраиваемых ролях и группах управления см. в статье [организация ресурсов с помощью групп управления Azure](../governance/management-groups/overview.md#azure-custom-role-definition-and-assignment).

## <a name="transferring-a-subscription-to-a-different-directory"></a>Передача подписки в другой каталог

- Сведения о том, как передавать подписку в другой каталог Azure AD, см. в статье [Перенос подписки Azure в другой каталог Azure AD](transfer-subscription.md).
- Если вы переносите подписку в другой каталог Azure AD, все назначения ролей **окончательно** удаляются из исходного каталога Azure AD и не переносятся в целевой каталог Azure AD. Необходимо повторно создать назначения ролей в целевом каталоге. Также необходимо вручную создать управляемые удостоверения для ресурсов Azure. Дополнительные сведения см. в разделе [часто задаваемые вопросы и известные проблемы с управляемыми удостоверениями](../active-directory/managed-identities-azure-resources/known-issues.md).
- Если вы являетесь глобальным администратором Azure AD и у вас нет доступа к подписке после ее передачи между каталогами, используйте переключатель **Управление доступом для ресурсов Azure** , чтобы временно [повысить уровень доступа](elevate-access-global-admin.md) , чтобы получить доступ к подписке.

## <a name="issues-with-service-admins-or-co-admins"></a>Проблемы с администраторами или соадминистраторами служб

- Если у вас возникли проблемы с администратором служб или соадминистратором, см. статью [Добавление или изменение администраторов Подписки Azure](../cost-management-billing/manage/add-change-subscription-administrator.md) и [ролей администратора классической подписки, ролей Azure и ролей Azure AD](rbac-and-directory-admin-roles.md).

## <a name="access-denied-or-permission-errors"></a>Отказ в доступе или ошибках разрешений

- Если при попытке создать ресурс появляется сообщение об ошибке разрешения "Клиент с идентификатором объекта не авторизован для выполнения действия с областью (код: AuthorizationFailed)", убедитесь, что вы вошли как пользователь, которому назначена роль с разрешением на запись для ресурса в выбранной области. Например, для управления виртуальными машинами в группе ресурсов вам должна быть назначена роль [Участник виртуальных машин](built-in-roles.md#virtual-machine-contributor) для этой группы ресурсов (или вышестоящего уровня). Список разрешений для каждой встроенной роли см. [в статье встроенные роли Azure](built-in-roles.md).
- Если при попытке создать или обновить запрос в службу поддержки появится сообщение об ошибке "у вас нет разрешения на создание запроса на поддержку", убедитесь, что вы вошли с пользователем, которому назначена роль с `Microsoft.Support/supportTickets/write` разрешением, например [участник запроса на поддержку](built-in-roles.md#support-request-contributor).

## <a name="move-resources-with-role-assignments"></a>Перемещение ресурсов с помощью назначений ролей

При перемещении ресурса с ролью Azure, назначенной непосредственно ресурсу (или дочернему ресурсу), назначение роли не перемещается и становится потерянным. После перемещения необходимо повторно создать назначение ролей. В конечном итоге назначение потерянной роли будет автоматически удалено, но рекомендуется удалить назначение ролей перед перемещением ресурса.

Дополнительные сведения о перемещении ресурсов см. в статье [Перемещение ресурсов в новую группу ресурсов или подписку](../azure-resource-manager/management/move-resource-group-and-subscription.md).

## <a name="role-assignments-with-identity-not-found"></a>Назначения ролей с удостоверением не найдены

В списке назначений ролей для портал Azure можно заметить, что субъект безопасности (пользователь, группа, субъект-служба или управляемое удостоверение) указан в списке **удостоверение не найдено** с **неизвестным** типом.

![Удостоверение не найдено в списке назначений ролей Azure](./media/troubleshooting/unknown-security-principal.png)

Удостоверение может быть не найдено по двум причинам:

- Вы недавно пригласили пользователя при создании назначения роли
- Вы удалили субъект безопасности с назначением роли

Если вы недавно пригласили пользователя при создании назначения роли, этот субъект безопасности может по-прежнему находиться в процессе репликации между регионами. В этом случае подождите несколько секунд и обновите список назначений ролей.

Однако если этот субъект безопасности не является недавно приглашенным пользователем, он может быть удаленным субъектом безопасности. Если назначить роль субъекту безопасности, а затем удалить этот субъект безопасности без предварительного удаления назначения ролей, то субъект безопасности будет указан как **удостоверение не найдено** и **неизвестный** тип.

Если вы перечислите назначение этой роли с помощью Azure PowerShell, то может отобразиться пустое `DisplayName` значение, а `ObjectType` для параметра задано **неизвестное**. Например, командлет [Get-азролеассигнмент](/powershell/module/az.resources/get-azroleassignment) Возвращает назначение роли, похожее на следующие выходные данные:

```
RoleAssignmentId   : /subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111
DisplayName        :
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 33333333-3333-3333-3333-333333333333
ObjectType         : Unknown
CanDelegate        : False
```

Аналогично, если вы перечислите назначение ролей с помощью Azure CLI, может отобразиться пустое значение `principalName` . Например, [AZ Role назначений List](/cli/azure/role/assignment#az-role-assignment-list) Возвращает назначение роли, похожее на следующие выходные данные:

```
{
    "canDelegate": null,
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222",
    "name": "22222222-2222-2222-2222-222222222222",
    "principalId": "33333333-3333-3333-3333-333333333333",
    "principalName": "",
    "roleDefinitionId": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
    "roleDefinitionName": "Storage Blob Data Contributor",
    "scope": "/subscriptions/11111111-1111-1111-1111-111111111111",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Не существует проблемы с тем, чтобы не выходить из этих назначений ролей, когда субъект безопасности был удален. При желании вы можете удалить эти назначения ролей, используя шаги, аналогичные другим назначениям ролей. Сведения об удалении назначений ролей см. в разделе [портал Azure](role-assignments-portal.md#remove-a-role-assignment), [Azure PowerShell](role-assignments-powershell.md#remove-a-role-assignment)или [Azure CLI](role-assignments-cli.md#remove-a-role-assignment)

В PowerShell при попытке удаления назначений ролей с помощью идентификатора объекта и имени определения роли, а также нескольких назначений ролей, соответствующих Вашим параметрам, вы получите сообщение об ошибке: "указанные сведения не соответствуют назначению роли". Ниже приведен пример сообщения об ошибке.

```
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor"

Remove-AzRoleAssignment : The provided information does not map to a role assignment.
At line:1 char:1
+ Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : CloseError: (:) [Remove-AzRoleAssignment], KeyNotFoundException
+ FullyQualifiedErrorId : Microsoft.Azure.Commands.Resources.RemoveAzureRoleAssignmentCommand
```

Если вы получаете это сообщение об ошибке, убедитесь, что вы также указали `-Scope` `-ResourceGroupName` Параметры или.

```
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor" - Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="role-assignment-changes-are-not-being-detected"></a>Изменения назначения ролей не обнаружены

Azure Resource Manager иногда кэширует конфигурации и данные для повышения производительности. При добавлении или удалении назначений ролей может потребоваться до 30 минут, чтобы изменения вступили в силу. Если вы используете портал Azure, Azure PowerShell или Azure CLI, можно принудительно обновить изменения назначения ролей, выйдя из учетной записи и повторно войдя в нее. Если вы вносите изменения в назначения ролей с помощью вызовов REST API, можно принудительно обновить изменения, обновив маркер доступа.

Если вы добавляете или удаляете назначение ролей в области группы управления, а роль имеет `DataActions` , то доступ к плоскости данных может не обновляться в течение нескольких часов. Это относится только к области группы управления и плоскости данных.

## <a name="web-app-features-that-require-write-access"></a>Компоненты веб-приложений, требующие доступа для записи

Если предоставить пользователю доступ только для чтения к одному веб-приложению, некоторые компоненты могут неожиданно отключиться. Перечисленные ниже возможности управления требуют доступа к веб-приложению с правом **записи** (роль участника или владельца), поэтому они недоступны при наличии прав только на чтение.

* Команды (такие как запуск, остановка и т. д.).
* Изменение параметров, таких как общие параметры конфигурации, параметры масштабирования, резервного копирования и мониторинга.
* Доступ к учетным данным для публикации и другим секретным сведениям, например к параметрам приложений и строкам подключения.
* Журналы потоковой передачи.
* Конфигурация журналов ресурсов
* Консоль (командная строка).
* Активные и недавние развертывания (для локального непрерывного развертывания Git).
* Приблизительные затраты.
* Веб-тесты
* Виртуальная сеть (видна читателю только в том случае, если ранее была настроена пользователем с доступом на запись).

Если у вас нет доступа ни к одной из этих плиток, попросите администратора предоставить вам доступ к веб-приложению с правами участника.

## <a name="web-app-resources-that-require-write-access"></a>Ресурсы веб-приложений, требующие доступа для записи

Работа с веб-приложениями осложняется наличием нескольких взаимосвязанных ресурсов. Вот типичная группа ресурсов, связанная с несколькими веб-сайтами:

![Группа ресурсов веб-приложений](./media/troubleshooting/website-resource-model.png)

В результате, если вы предоставите кому-либо доступ только к веб-приложению, многие функции в колонке веб-сайта на портале Azure будут отключены.

Для этих элементов требуется доступ на **запись** к **плану службы приложений**, который соответствует вашему веб-сайту:  

* просмотр ценовой категории веб-приложения (например, "Бесплатный" или "Стандартный");  
* параметры масштабирования (число экземпляров, размер виртуальной машины, настройки автоматического масштабирования);  
* квоты (квоты хранилища, пропускной способности, ресурсов ЦП).  

Элементы, требующие доступа на **запись** ко всей **группе ресурсов**, которая содержит веб-сайт:  

* Сертификаты и привязки TLS/SSL (сертификаты TLS/SSL могут совместно использоваться сайтами в одной группе ресурсов и в географическом расположении).  
* Правила оповещения  
* параметры автоматического масштабирования;  
* компоненты Application Insights;  
* Веб-тесты  

## <a name="virtual-machine-features-that-require-write-access"></a>Компоненты виртуальных машин, требующие доступа для записи

Как и в случае с веб-приложениями, для некоторых функций в колонке виртуальной машины нужен доступ к виртуальной машине или другим ресурсам в группе ресурсов с правами на запись.

Виртуальные машины связаны с доменными именами, виртуальными сетями, учетными записями хранения и правилами оповещений.

Элементы, требующие доступа к **виртуальной** машине с правом **записи**:

* Конечные точки  
* IP-адреса  
* Диски  
* Расширения  

Элементы, требующие доступа на **запись** как к **виртуальной машине**, так и к **группе ресурсов**, в которой она находится (а также к доменному имени):  

* Группа доступности  
* набор балансировки нагрузки;  
* Правила оповещения  

Если у вас нет доступа ни к одному из этих элементов, попросите администратора предоставить вам права участника для группы ресурсов.

## <a name="azure-functions-and-write-access"></a>Функции Azure и доступ для записи

Для некоторых компонентов решения [Функции Azure](../azure-functions/functions-overview.md) требуется доступ на запись. Например, если пользователю назначена роль [читателя](built-in-roles.md#reader) , он не сможет просматривать функции в приложении-функции. На портале отобразится текст **(Нет доступа)**.

![Сообщение об отсутствии доступа в приложении-функции](./media/troubleshooting/functionapps-noaccess.png)

Пользователь с ролью читателя может щелкнуть вкладку **Функции платформы** и выбрать **Все параметры**, чтобы просмотреть некоторые параметры, связанные с приложением-функцией (так же, как для веб-приложения), но не может изменить эти параметры. Для доступа к этим возможностям потребуется роль [участника](built-in-roles.md#contributor) .

## <a name="next-steps"></a>Дальнейшие действия

- [Устранение неполадок для гостевых пользователей](role-assignments-external-users.md#troubleshoot)
- [Добавление и удаление назначений ролей Azure с помощью портала Azure](role-assignments-portal.md)
- [Просмотр журналов действий для изменений Azure RBAC](change-history-report.md)
