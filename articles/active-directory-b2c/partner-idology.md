---
title: Интеграция Идологи с Azure Active Directory B2C
titleSuffix: Azure AD B2C
description: Узнайте, как интегрировать пример веб-приложения оплаты в Azure AD B2C с Идологи. Идологи — это поставщик проверки и проверки личности с несколькими решениями.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/08/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 07a8f785cf2b9a64f3acb9f44c4fca5023c4fcf3
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "94953752"
---
# <a name="tutorial-for-configuring-idology-with-azure-active-directory-b2c"></a>Руководство по настройке Идологи с помощью Azure Active Directory B2C 

В этом примере учебника мы предоставляем рекомендации по интеграции Azure AD B2C с [идологи](https://www.idology.com/solutions/). Идологи — это поставщик проверки и проверки личности с несколькими решениями. В этом примере мы рассмотрим Експектид решение с помощью Идологи.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, вам потребуется:

* подписка Azure AD; Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* [Клиент Azure AD B2C](tutorial-create-tenant.md) , связанный с вашей подпиской Azure.

## <a name="scenario-description"></a>Описание сценария

Интеграция Идологи включает следующие компоненты:

- Azure AD B2C — сервер авторизации, отвечающий за проверку учетных данных пользователя. Он также известен как поставщик удостоверений.
- Идологи — служба Идологи принимает входные данные, предоставленные пользователем, и проверяет подлинность пользователя.
- Пользовательский API-интерфейс службы "интерфейс пользователя" — Этот API реализует интеграцию между Azure AD и службой Идологи.

Реализация показана на следующей схеме архитектуры.

![Схема архитектуры Идологи](media/partner-idology/idology-architecture-diagram.png)

| Шаг | Описание |
|------|------|
|1     | Пользователь поступает на страницу входа. |
|2     | Пользователь выбирает вариант регистрации для создания новой учетной записи и ввода данных на странице. Azure AD B2C собирает атрибуты пользователя. |
|3     | Azure AD B2C вызывает API среднего уровня и передает атрибуты пользователя. |
|4     | API среднего уровня собирает атрибуты пользователя и преобразует их в формат, который может использовать API Идологи. Затем он отправляет сведения в Идологи. |
|5     | Идологи потребляет информацию и обрабатывает ее, а затем возвращает результат в API среднего уровня. |
|6     | API среднего уровня обрабатывает сведения и отправляет соответствующие сведения обратно в Azure AD B2C. |
|7     | Azure AD B2C получает сведения от API среднего уровня. Если отображается ответ на **сбой** , пользователю выдается сообщение об ошибке. Если отображается ответ **об успешном выполнении** , пользователь проходит проверку подлинности и записывается в каталог. |
|      |      |

> [!NOTE]
> Azure AD B2C также может попросить клиента выполнить пошаговую проверку подлинности, но этот сценарий выходит за рамки этого руководства.

## <a name="onboard-with-idology"></a>Подключение с помощью Идологи

1. Идологи предоставляет разнообразные решения, которые можно найти [здесь](https://www.idology.com/solutions/). Для этого примера мы используем Експектид.

2. Чтобы создать учетную запись Идологи, обратитесь в [идологи](https://www.idology.com/request-a-demo/microsoft-integration-signup/).

3. После создания учетной записи вы получите сведения, необходимые для настройки API. В следующих разделах описывается процесс.

## <a name="integrate-with-azure-ad-b2c"></a>Интеграция с Azure AD B2C

### <a name="part-1---deploy-the-api"></a>Часть 1. Развертывание API

Разверните предоставленный [код API](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/IDology/Api) в службе Azure. Код можно опубликовать из Visual Studio, следуя этим [инструкциям](/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019).

Вам потребуется URL-адрес развернутой службы, чтобы настроить Azure AD с использованием требуемых параметров.

### <a name="part-2---configure-the-api"></a>Часть 2. Настройка API 

Параметры приложения можно [настроить в службе приложений в Azure](../app-service/configure-common.md#configure-app-settings). С помощью этого метода параметры можно безопасно настроить без их возврата в репозиторий. Необходимо предоставить следующие параметры для API-интерфейса RESTful:

| Параметры приложений | Источник | Примечания |
| :-------- | :------------| :-----------|
|Идологисеттингс: Апиусернаме | Конфигурация учетной записи Идологи |     |
|Идологисеттингс: Апипассворд | Конфигурация учетной записи Идологи |     |
|Вебаписеттингс: Апиусернаме |Определение имени пользователя для API| Используется в конфигурации Екстид |
|Вебаписеттингс: Апипассворд | Определение пароля для API | Используется в конфигурации Екстид

### <a name="part-3---create-api-policy-keys"></a>Часть 3. Создание ключей политики API

Следуйте этому [документу](secure-rest-api.md#add-rest-api-username-and-password-policy-keys) , чтобы создать два ключа политики: один для имени пользователя API и один для пароля API, определенного выше.

В образце политики используются следующие имена ключей:

* B2C_1A_RestApiUsername
* B2C_1A_RestApiPassword

### <a name="part-4---configure-the-azure-ad-b2c-policy"></a>Часть 4. Настройка политики Azure AD B2C

1. Следуйте указаниям в этом [документе](custom-policy-get-started.md?tabs=applications#custom-policy-starter-pack) , чтобы скачать [начальный пакет LocalAccounts](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts) и настроить политику для клиента Azure AD B2C. Следуйте инструкциям, пока не будет выполнена **Проверка раздела настраиваемая политика** .

2. Загрузите два [примера политик.](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/IDology/policy)

3. Обновите два примера политик:

   1. Откройте обе политики:

      1. В разделе `Idology-ExpectId-API` обновите `ServiceUrl` элемент метаданных, указав расположение ранее развернутого API.

      1. Замените `yourtenant` именем вашего клиента Azure AD B2C.
      Например, если имя клиента Azure AD B2C —  `contosotenant` , замените все экземпляры  `yourtenant.onmicrosoft.com`   на `contosotenant.onmicrosoft.com` .

   1. Откройте файл TrustFrameworkExtensions.xml:

      1. Найдите элемент  `<TechnicalProfile Id="login-NonInteractive">` . Замените оба экземпляра  `IdentityExperienceFrameworkAppId`   идентификатором приложения приложения IdentityExperienceFramework, созданного ранее.

      1. Замените оба экземпляра  `ProxyIdentityExperienceFrameworkAppId`   идентификатором приложения приложения ProxyIdentityExperienceFramework, созданного ранее.

4. Замените SignInorSignUp.xml и TrustFrameworkExtensions.xml ранее отправленные на Azure AD B2C на шаге 1 двумя обновленными примерами политик.

> [!NOTE]
> Рекомендуется, чтобы клиенты добавили уведомление о согласия на страницу коллекции атрибутов. Уведомлять пользователей о том, что сведения будут отправляться сторонним службам для проверки личности.

## <a name="test-the-user-flow"></a>Тестирование потока пользователя

1. Откройте клиент Azure AD B2C и в разделе **политики** выберите **потоки пользователей**.

2. Выберите ранее созданный **поток пользователя**.

3. Выберите **запустить поток пользователя** и выберите параметры:

   1. **Приложение** — выберите зарегистрированное приложение (пример — JWT).

   1. **URL-адрес ответа** — выберите **URL-адрес перенаправления**.

   1. Выберите **Выполнить поток пользователя**.

4. Пройдите процесс регистрации и создайте учетную запись.

5. Выполните выход.

6. Пройдите по потоку входа.

7. После ввода **продолжения** появится головоломка идологи.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих статьях:

- [Пользовательские политики в Azure AD B2C](custom-policy-overview.md)

- [Приступая к работе с пользовательскими политиками в Azure AD B2C](custom-policy-get-started.md?tabs=applications)