---
title: Квоты в предварительной версии Статических веб-приложений
description: Узнайте о квотах, связанных с предварительной версией Статических веб-приложений
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 915f8675ffda0d70347905d11c7d93975b9d4526
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132743"
---
# <a name="quotas-in-azure-static-web-apps-preview"></a>Квоты в предварительной версии Статических веб-приложений

Для предварительной версии Статических веб-приложений существуют следующие квоты.

> [!IMPORTANT]
> Предварительная версия Статических веб-приложений не предназначена для использования в рабочей среде.

| Компонент                     | План "Бесплатный"        |
|-----------------------------|------------------|
| Предоставляемая пропускная способность          | 100 ГБ в месяц |
| Избыточная пропускная способность           | Рекомендации недоступны      |
| Приложений на подписку Azure | 10               |
| Размер приложения                    | 250 МБ           |
| Подготовительные среды | 1                |
| Личные домены              | 1                |
| Авторизация<br><br>С пользовательскими ролями и правилами маршрутизации | Максимум 25 приглашенных пользователей и назначенных ролей |
| Функции Azure             | Доступно        |
| Соглашение об уровне обслуживания                         | None             |

## <a name="github-storage"></a>Хранилище GitHub

GitHub Actions и GitHub Packages используют хранилище GitHub с собственным набором квот. Когда вы создаете сайт статических веб-приложений, GitHub хранит артефакты для сайта даже после развертывания.

Для получения дополнительных сведений см. следующие ресурсы:

- [Управление дисковым пространством GitHub Actions](https://github.community/t5/GitHub-Actions/Managing-Actions-storage-space/td-p/38944)
- [Сведения о выставлении счетов за GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions#about-billing-for-github-actions)
- [Управление предельной суммой расходов для GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)

## <a name="next-steps"></a>Дальнейшие действия

- [Обзор](overview.md)
