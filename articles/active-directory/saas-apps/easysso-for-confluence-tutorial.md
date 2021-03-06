---
title: Руководство по интеграции единого входа Azure Active Directory с EasySSO for Confluence | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в EasySSO for Confluence.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/28/2020
ms.author: jeedes
ms.openlocfilehash: f37c036e144cf20a1ff217cb1bfb626ddff1b59e
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2020
ms.locfileid: "92454377"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-confluence"></a>Руководство по интеграции единого входа Azure Active Directory с EasySSO for Confluence

В этом учебнике описано, как интегрировать EasySSO for Confluence с Azure Active Directory (Azure AD). Интеграция EasySSO for Confluence с Azure AD обеспечивает следующие возможности:

* С помощью Azure AD вы можете контролировать доступ к Confluence.
* Можно разрешить автоматический вход пользователей в Confluence с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

Чтобы узнать больше об интеграции приложений SaaS с Azure AD, прочитайте статью [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка EasySSO for Confluence с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* EasySSO for Confluence поддерживает вход, инициированный **поставщиком услуг и поставщиком удостоверений** .
* EasySSO for Confluence поддерживает **JIT** -подготовку пользователей.
* После настройки EasySSO for Confluence вы можете применить функцию управления сеансом, которая защищает от хищения данных и несанкционированного доступа к конфиденциальным данным вашей организации в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-easysso-for-confluence-from-the-gallery"></a>Добавление EasySSO for Confluence из коллекции

Чтобы настроить интеграцию EasySSO for Confluence с Azure AD, необходимо добавить EasySSO for Confluence из коллекции в список управляемых приложений SaaS.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory** .
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения** .
1. Чтобы добавить новое приложение, выберите **Новое приложение** .
1. В разделе **Добавление из коллекции** в поле поиска введите **EasySSO for Confluence** .
1. Выберите **EasySSO for Confluence** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-confluence"></a>Настройка и проверка единого входа Azure Active Directory для EasySSO for Confluence

Настройте и проверьте единый вход Azure AD в EasySSO for Confluence с помощью тестового пользователя **B. Simon** . Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в EasySSO for Confluence.

Чтобы настроить и проверить единый вход Azure AD в EasySSO for Confluence, выполните действия из следующих стандартных блоков.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в EasySSO for Confluence](#configure-easysso-for-confluence-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    1. **[Создание тестового пользователя EasySSO for Confluence](#create-easysso-for-confluence-test-user)** требуется для того, чтобы в EasySSO for Confluence существовал пользователь B. Simon, связанный с одноименным пользователем в Azure AD.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **EasySSO for Confluence** найдите раздел **Управление** и выберите **Единый вход** .
1. На странице **Выбрать метод единого входа** выберите **SAML** .
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений** , в разделе **Базовая конфигурация SAML** введите значения следующих полей.

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<server-base-url>/plugins/servlet/easysso/saml`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<server-base-url>/plugins/servlet/easysso/saml`.

1. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг** , щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<server-base-url>/login.jsp`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Чтобы получить эти значения, обратитесь в [группу поддержки клиентов EasySSO](mailto:support@techtime.co.nz). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

1. Приложение EasySSO for Confluence ожидает проверочные утверждения SAML в определенном формате, поэтому следует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию.

    ![Изображение](common/default-attributes.png)

1. В дополнение к описанному выше приложение EasySSO for Confluence ожидает несколько дополнительных атрибутов в ответе SAML, как показано ниже. Эти атрибуты также заранее заполнены, но вы можете изменить их в соответствии со своими требованиями.
    
    | Имя | Исходный атрибут|
    | ---------------| --------- |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname |
    | urn:oid:0.9.2342.19200300.100.1.3 | user.mail |
    | urn:oid:2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:2.5.4.42 | user.givenname |
    
    Если у пользователей Azure AD настроен **sAMAccountName** , сопоставьте **urn:oid:0.9.2342.19200300.100.1.1** с атрибутом **sAMAccountName** .
    
1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку **Скачать** , чтобы скачать **Сертификат (Base64)** или **XML метаданных федерации** , и сохраните один из них или оба на компьютере. Они также потребуются позже, чтобы настроить EasySSO для Confluence.

    ![Ссылка для скачивания сертификата](./media/easysso-for-confluence-tutorial/certificate.png)
    
    Если вы планируете выполнить настройку EasySSO для Confluence вручную с помощью сертификата, необходимо также скопировать **URL-адрес входа** и **идентификатор Azure AD** в разделе ниже и сохранить их на компьютере.

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory** , **Пользователи** , а затем — **Все пользователи** .
1. В верхней части экрана выберите **Новый пользователь** .
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль** .
   1. Нажмите кнопку **Создать** .

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к EasySSO for Confluence.

1. На портале Azure выберите **Корпоративные приложения** , а затем — **Все приложения** .
1. Из списка приложений выберите **EasySSO for Confluence** .
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы** .

   ![Ссылка "Пользователи и группы"](common/users-groups-blade.png)

1. Выберите **Добавить пользователя** , а в диалоговом окне **Добавление назначения**  выберите **Пользователи и группы** .

    ![Ссылка "Добавить пользователя"](common/add-assign-user.png)

1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать** .
1. Если ожидается, что в утверждении SAML будет получено какое-либо значение роли, то в диалоговом окне **Выбор роли** нужно выбрать соответствующую роль для пользователя из списка и затем нажать кнопку **Выбрать** , расположенную в нижней части экрана.
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить** .

## <a name="configure-easysso-for-confluence-sso"></a>Настройка единого входа в EasySSO for Confluence

1. Войдите в экземпляр Atlassian Confluence с правами администратора и перейдите к разделу **Manage Apps** (Управление приложениями). 

    ![Manage Apps (Управление приложениями)](./media/easysso-for-confluence-tutorial/confluence-admin-1.png)

2. На левой стороне найдите пункт **EasySSO** и щелкните его. Затем нажмите кнопку **Configure** (Настройка).

    ![Easy SSO](./media/easysso-for-confluence-tutorial/confluence-admin-2.png)

3. Выберите параметр **SAML** . Вы перейдете к разделу конфигурации SAML.

    ![SAML](./media/easysso-for-confluence-tutorial/confluence-admin-3.png)

4. В верхней части окна выберите **Сертификаты** , и отобразится следующий экран: 

    ![URL-адрес метаданных](./media/easysso-for-confluence-tutorial/confluence-admin-4.png)

5. Теперь найдите **Сертификат (Base64)** или **Файл метаданных** , сохраненный на предыдущих шагах конфигурации **единого входа Azure AD** . Далее можно воспользоваться одним из следующих вариантов:

    а. Используйте **файл метаданных** федерации приложений, загруженный в локальный файл на компьютере. Выберите переключатель **Upload** (Отправить) и выполните инструкции в диалоговом окне отправки файла, которые зависят от вашей операционной системы.

    **OR**

    b. Откройте **файл метаданных** федерации приложений, чтобы просмотреть содержимое файла (в любом редакторе обычного текста) и скопировать его в буфер обмена. Выберите параметр **Input** (Входные данные) и вставьте содержимое буфера обмена в текстовое поле.
 
    **OR**

    c. Полная настройка вручную. Откройте **сертификат (Base64)** федерации приложений, чтобы просмотреть содержимое файла (в любом редакторе обычного текста) и скопировать его в буфер обмена. Вставьте файл в текстовое поле **IdP Token Signing Certificates** (IdP-сертификаты для подписи маркеров). Затем перейдите на вкладку **General** (Общие) и заполните поля **POST Binding URL** (URL-адрес привязки POST) и **Entity ID** (Идентификатор сущности) соответствующими значениями для **URL-адреса входа** и **идентификатора Azure AD** , сохраненных ранее.
 
6. В нижней части страницы нажмите кнопку **Save** (Сохранить). Содержимое файлов метаданных или сертификатов анализируется в полях конфигурации. Настройка EasySSO for Confluence завершена.

7. Для лучшего тестирования перейдите на вкладку **Look & Feel** (Дизайн и функциональность) и установите флажок **SAML Login Button** (Кнопка входа SAML). При этом на экране входа в Confluence будет активирована отдельная кнопка, специально предназначенная для тестирования сквозной интеграции SAML в Azure AD. Эту кнопку можно оставить включенной, а также настроить ее размещение, цвет и преобразовать ее для рабочего режима.

    ![Вкладка Look & Feel (Дизайн и функциональность)](./media/easysso-for-confluence-tutorial/confluence-admin-5.png)

    > [!NOTE]
    > Если у вас возникнут проблемы, обратитесь в [группу поддержки EasySSO](mailto:support@techtime.co.nz).

### <a name="create-easysso-for-confluence-test-user"></a>Создание тестового пользователя EasySSO for Confluence

В этом разделе показано, как создать в Confluence пользователя Britta Simon. Приложение EasySSO for Confluence поддерживает JIT-подготовку пользователей, которая **отключена** по умолчанию. Чтобы включить подготовку пользователей, необходимо в разделе General (Общие) подключаемого модуля конфигурации EasySSO явно установить флажок **Create user on successful login** (Создать пользователя при успешном входе). Если пользователь еще не существует в Confluence, он создается после проверки подлинности.

Но если вы не хотите включать автоматическую подготовку пользователей при первом входе в систему, они должны существовать во внутренних каталогах пользователей, которые использует экземпляр Confluence, например LDAP или Atlassian Crowd.

![Подготовка пользователей](./media/easysso-for-confluence-tutorial/confluence-admin-6.png)

## <a name="test-sso"></a>Проверка единого входа 

### <a name="idp-initiated-workflow"></a>Рабочий процесс, инициированный поставщиком удостоверений

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку EasySSO для Confluence на Панели доступа, вы автоматически войдете в экземпляр Confluence, для которого настроили единый вход. См. дополнительные сведения о [панели доступа](../user-help/my-apps-portal-end-user-access.md)

### <a name="sp-initiated-workflow"></a>Рабочий процесс, инициированный поставщиком услуг

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью кнопки **SAML Login** (Вход SAML) в Confluence.

![Вход SAML для пользователей](./media/easysso-for-confluence-tutorial/confluence-admin-7.png)

В этом сценарии предполагается, что вы включили параметр **SAML Login Button** (Кнопка входа SAML) на вкладке **Look & Feel** (Дизайн и функциональность) на странице конфигурации EasySSO для Confluence (см. выше). Откройте URL-адрес входа Confluence в браузере в режиме инкогнито, чтобы избежать пересечений с существующими сеансами. Нажмите кнопку **Вход SAML** , и вы будете перенаправлены в поток проверки подлинности пользователя Azure AD. После успешного завершения вы будете перенаправлены к экземпляру Confluence в качестве пользователя, прошедшего проверку подлинности с помощью SAML.

Есть вероятность, что после перенаправления обратно из Azure AD вы увидите следующий экран:

![Экран сбоя EasySSO](./media/easysso-for-confluence-tutorial/confluence-admin-8.png)

В этом случае необходимо выполнить [инструкции на этой странице]( https://techtime.co.nz/display/TECHTIME/EasySSO+How+to+get+the+logs#EasySSOHowtogetthelogs-RETRIEVINGTHELOGS), чтобы получить доступ к файлу **atlassian-confluence.log** . Подробные сведения об этой ошибке см. по ссылочному идентификатору на странице ошибки EasySSO.

Если у вас возникнут проблемы с приемом сообщений журнала, обратитесь в [группу поддержки EasySSO](mailto:support@techtime.co.nz).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Список учебников по интеграции приложений SaaS с Azure Active Directory](./tutorial-list.md)

- [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Что представляет собой условный доступ в Azure Active Directory?](../conditional-access/overview.md)

- [Попробуйте использовать EasySSO for Confluence с Azure AD](https://aad.portal.azure.com/)

- [Что такое управление сеансами в Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Защита приложений с помощью функции управления настройками условного доступа для приложений в Microsoft Cloud App Security](/cloud-app-security/proxy-intro-aad)