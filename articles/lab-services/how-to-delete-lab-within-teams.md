---
title: Удаление лаборатории лабораторных служб Azure из рабочих групп
description: Узнайте, как удалить лабораторию служб лаборатории Azure из рабочих групп.
ms.topic: article
ms.date: 10/12/2020
ms.openlocfilehash: 8d1e20f8f676eb9863187b550a3c0400871d670c
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96433960"
---
# <a name="delete-labs-within-teams"></a>Удаление лабораторий в группах

В этой статье показано, как удалить лабораторию из приложения **служб лабораторий Azure** .

## <a name="prerequisites"></a>Предварительные требования

* [Создайте учетную запись служб лаборатории](tutorial-setup-lab-account.md#create-a-lab-account) в портал Azure.
* Начните [работу и создайте лабораторию лабораторных служб в группах](how-to-get-started-create-lab-within-teams.md).

## <a name="delete-labs"></a>Удаление лабораторий

Лабораторные работы, созданные в командах, можно удалить на [веб-сайте служб лаборатории](https://labs.azure.com) , непосредственно удалив лабораторию, как описано в статье [Управление лабораториями в службах лаборатории Azure](how-to-manage-classroom-labs.md). 

Удаление лаборатории также активируется при удалении команды. Если группа, в которой создана лаборатория, удаляется, лаборатория будет автоматически удалена через 24 часа после запуска автоматической синхронизации списка пользователей. 

> [!IMPORTANT]
> Удаление вкладки или удаления приложения не приведет к удалению лаборатории. 

Если вкладка удалена, пользователи в списке членства в группе будут по-прежнему иметь доступ к виртуальным машинам на [веб-сайте служб лаборатории](https://labs.azure.com) , пока удаление лаборатории не будет явно активировано путем удаления лаборатории на веб-сайте или удаления команды. 

## <a name="next-steps"></a>Дальнейшие действия

- [Обзор использования служб лаборатории Azure в рабочих группах](lab-services-within-teams-overview.md)
- [Управление списками пользователей лабораторий в командах](how-to-manage-user-lists-within-teams.md)
- [Управление пулом виртуальных машин лаборатории в рамках команд](how-to-manage-vm-pool-within-teams.md)
- [Создание и управление расписаниями лаборатории в рабочих группах](how-to-create-schedules-within-teams.md)
- [Доступ к виртуальной машине в командах — представление учащихся](how-to-access-vm-for-students-within-teams.md)

