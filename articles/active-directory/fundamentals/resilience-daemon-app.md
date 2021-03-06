---
title: Повышение устойчивости проверки подлинности и авторизации в разрабатываемых управляющих приложениях
titleSuffix: Microsoft identity platform
description: Рекомендации по повышению устойчивости проверки подлинности и авторизации в управляющем приложении с использованием платформы идентификации Майкрософт
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: 74bfc9eeeb8375fca2c88a3fd3c31f17e130fc99
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919995"
---
# <a name="increase-the-resilience-of-authentication-and-authorization-in-daemon-applications-you-develop"></a>Повышение устойчивости проверки подлинности и авторизации в разрабатываемых управляющих приложениях

В этой статье содержатся рекомендации по использованию платформы идентификации Майкрософт и Azure Active Directory для повышения устойчивости управляющих приложений. Сюда входят фоновые процессы, службы, серверные приложения и приложения без пользователей.

![Управляющее приложение, выполняющее вызов Microsoft Identity](media/resilience-daemon-app/calling-microsoft-identity.png)

## <a name="use-managed-identities-for-azure-resources"></a>Использование управляемых удостоверений для ресурсов Azure

Разработчики, создающие управляющие приложения на Microsoft Azure могут использовать [управляемые удостоверения для ресурсов Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Управляемые удостоверения устраняют необходимость в управлении секретами и учетными данными для разработчиков. Эта функция повышает устойчивость, избегая ошибок, связанных с истечением срока действия сертификата, ошибками вращения или доверием. Он также имеет несколько встроенных функций, специально предназначенных для повышения устойчивости.

Управляемые удостоверения используют долгосрочные маркеры доступа и сведения от Microsoft Identity для упреждающего получения новых маркеров в течение большого периода времени до истечения срока действия существующего маркера. Приложение может продолжать работу при попытке получить новый маркер.

Управляемые удостоверения также используют региональные конечные точки для повышения производительности и устойчивости к сбоям вне региона. Использование региональной конечной точки позволяет удерживать весь трафик внутри географической области. Например, если ресурс Azure находится в WestUS2, весь трафик, включая трафик, созданный удостоверением Майкрософт, должен остаться в WestUS2. Это устраняет возможные точки сбоя, объединяя зависимости службы.

## <a name="use-the-microsoft-authentication-library"></a>Использование библиотеки проверки подлинности Майкрософт

Разработчики управляющих приложений, которые не используют управляемые удостоверения, могут использовать [библиотеку проверки подлинности Майкрософт (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview), которая упрощает реализацию проверки подлинности и авторизации и автоматически использует рекомендации по обеспечению устойчивости. MSAL сделает процесс предоставления требуемых клиентских учетных данных проще. Например, приложению не требуется реализовывать создание и подписывание утверждений JSON Web Token при использовании учетных данных на основе сертификатов.

### <a name="use-microsoftidentityweb-for-net-developers"></a>Использование Microsoft. Identity. Web для разработчиков .NET

Разработчики, создающие управляющие приложения на ASP.NET Core могут использовать библиотеку [Microsoft. Identity. Web](https://docs.microsoft.com/azure/active-directory/develop/microsoft-identity-web) . Эта Библиотека построена на основе MSAL, чтобы упростить реализацию авторизации для ASP.NET Core приложений. Он включает несколько стратегий [кэша распределенных маркеров](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization#distributed-token-cache) для распределенных приложений, которые могут работать в нескольких регионах.

## <a name="cache-and-store-tokens"></a>Кэширование и хранение токенов

Если вы не используете MSAL для реализации проверки подлинности и авторизации, можно реализовать некоторые рекомендации по кэшированию и хранению маркеров. MSAL реализует и использует эти рекомендации автоматически.

Приложение получает маркеры от поставщика удостоверений, чтобы авторизовать приложение для вызова защищенных интерфейсов API. Когда приложение получает маркеры, ответ, содержащий маркеры, также содержит свойство с истекшим сроком действия \_ , которое сообщает приложению, как долго кэшировать и повторно использовать маркер. Важно, чтобы в приложениях использовалось свойство «Expires \_ » для определения времени существования маркера. Приложение не должно пытаться декодировать маркер доступа API. Использование кэшированного маркера предотвращает ненужный трафик между приложением и удостоверением Майкрософт. Пользователь может оставаться в вашем приложении в течение времени существования этого маркера.

## <a name="properly-handle-service-responses"></a>Правильная обработка ответов службы

Наконец, хотя приложения должны поддерживать все ответы на ошибки, существуют ответы, которые могут повлиять на устойчивость. Если приложение получает код ответа HTTP 429, а также слишком много запросов, Microsoft Identity выполняет регулирование запросов. Если приложение продолжает создавать слишком много запросов, оно будет по прежнему регулироваться, предотвращая получение маркеров приложением. Приложение не должно попытаться получить маркер еще раз, пока не пройдет время в секундах в поле ответа "повторить попытку". Получение ответа 429 часто свидетельствует о том, что приложение не кэширует и повторно использует токены правильно. Разработчики должны проанализировать кэширование и повторное использование маркеров в приложении.

Когда приложение получает код ответа HTTP 5xx, приложение не должно входить в цикл быстрого повтора. Если этот параметр существует, приложение должно соблюдать ту же обработку "повтора после", что и для ответа 429. Если ответ не предоставляет заголовок "Retry-After", рекомендуется выполнить повторную попытку, выполнив ее с первой попыткой по крайней мере через 5 секунд после ответа.

Если время ожидания запроса истекло, повторная попытка выполнения приложения невозможна. Реализовать экспоненциальную повторную попытку с первой повторной попыткой по крайней мере через 5 секунд после ответа.

## <a name="next-steps"></a>Дальнейшие действия

- [Создание устойчивости для приложений, которые входят в систему пользователей](resilience-client-app.md)
- [Отказоустойчивость сборок в инфраструктуре управления удостоверениями и доступом](resilience-in-infrastructure.md)
- [Устойчивость сборок в CIAM Systems](resilience-b2c.md)
