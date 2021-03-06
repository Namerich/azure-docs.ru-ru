---
title: Создание приложения-функции с помощью развертывания GitHub — Azure CLI
description: Создайте приложение-функцию и разверните код функции из репозитория GitHub с помощью решения "Функции Azure".
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 752d8a729c56fc2e0e110fcedc263eab4ba7a96b
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565319"
---
# <a name="create-a-function-app-in-azure-that-is-deployed-from-github"></a>Создание в Azure приложения-функции, которое развертывается из GitHub

В этом примере скрипта, написанного при помощи решения "Функции Azure", создается приложение-функция с использованием [плана потребления](../functions-scale.md#consumption-plan) и связанные с ним ресурсы. Код функции настраивается для непрерывного развертывания из репозитория GitHub. 

Для этого примера вам потребуются следующие компоненты:

* Репозиторий GitHub с кодом функции, для которого у вас есть права администратора.
* [Личный маркер доступа](https://help.github.com/articles/creating-an-access-token-for-command-line-use) для учетной записи GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Для работы с этим руководством требуется Azure CLI версии 2.0 или более поздней. Если вы используете Azure Cloud Shell, последняя версия уже установлена. 

## <a name="sample-script"></a>Пример скрипта

Этот пример создает приложения-функцию Azure и развертывает код функции из GitHub.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

Для каждой команды в таблице приведены ссылки на соответствующую документацию. Этот сценарий использует следующие команды:

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Создает учетную запись хранения, необходимую для приложения-функции. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Создает приложение-функцию в бессерверном [плане потребления](../functions-scale.md#consumption-plan) и связывает его с репозиторием Git или Mercurial. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры сценариев Azure CLI для Функций Azure см. в [документации по Функциям Azure](../functions-cli-samples.md).
