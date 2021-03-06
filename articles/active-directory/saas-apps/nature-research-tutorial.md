---
title: Руководство по интеграции единого входа Azure Active Directory с Nature Research | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении Nature Research.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/29/2020
ms.author: jeedes
ms.openlocfilehash: d68305087264d6ba2306ba4c345064693b595c94
ms.sourcegitcommit: bbd66b477d0c8cb9adf967606a2df97176f6460b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2020
ms.locfileid: "93234024"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nature-research"></a>Руководство по интеграции единого входа Azure Active Directory с Nature Research

В этом учебнике описано, как интегрировать Nature Research с Azure Active Directory (Azure AD). Интеграция Nature Research с Azure AD обеспечивает следующие возможности.

* Контроль доступа к Nature Research с помощью Azure AD.
* Автоматический вход пользователей в Nature Research с помощью учетных записей Azure AD.
* Централизованное управление учетными записями через портал Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, потребуется следующее.

* Подписка Azure AD. Если у вас нет подписки, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/).
* Подписка Nature Research с поддержкой единого входа.

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение Nature Research поддерживает единый вход, инициированный **поставщиком услуг и поставщиком удостоверений**.

## <a name="adding-nature-research-from-the-gallery"></a>Добавление Nature Research из коллекции

Чтобы настроить интеграцию приложения Nature Research с Azure AD, вам нужно добавить это приложение из коллекции в свой список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Nature Research**.
1. Выберите **Nature Research** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-nature-research"></a>Настройка и проверка единого входа Azure AD для Nature Research

Настройте и проверьте единый вход Azure AD в Nature Research с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Nature Research.

Чтобы настроить и проверить единый вход Azure AD в Nature Research, выполните действия из следующих стандартных блоков:

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    1. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD с помощью пользователя B.Simon.
    1. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить пользователю B.Simon использовать единый вход Azure AD.
1. **[Настройка единого входа в Nature Research](#configure-nature-research-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
1. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Nature Research** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, для этого в разделе **Базовая конфигурация SAML** предварительно заданы значения идентификатора и URL-адреса ответа в Azure. Однако вам нужно ввести значение состояния ретранслятора.

    В текстовое поле **Состояние ретранслятора** введите такой URL-адрес: `https://www.nature.com`.  
    Щелкните **Сохранить**.

1. Чтобы настроить приложение в инициируемом режиме **пакета обновления**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://sp.nature.com/saml/login?idp=<IDP_ENTITY_ID>`.

    > [!NOTE]
    > Значение URL-адреса входа приведено для примера. `<IDP_ENTITY_ID>` — это идентификатор Azure AD, скопированный из раздела **Настройка Nature Research**. Можно также посмотреть шаблоны в разделе "Базовая конфигурация SAML" на портале Azure.

1. Выберите команду **Сохранить**.

1. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы скопировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к Nature Research.

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. Из списка приложений выберите **Nature Research**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-nature-research-sso"></a>Настройка единого входа в Nature Research

Для настройки единого входа на стороне **Nature Research** необходимо отправить **URL-адрес метаданных федерации приложений** в [группу поддержки Nature Research](mailto:onlineservice@springernature.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в Nature Research, где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в Nature Research и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* На портале Azure выберите **Тестировать приложение**, и вы автоматически войдете в приложение Nature Research, для которого настроен единый вход. 

Вы можете также использовать Панель доступа корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку Nature Research на Панели доступа, вы перейдете на страницу входа приложения, если выполнены настройки для использования в режиме поставщика услуг, или автоматически войдете в приложение Nature Research, для которого настроен единый вход, если выполнены настройки для использования в режиме поставщика удостоверений. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)


## <a name="next-steps"></a>Дальнейшие действия

После настройки Nature Research вы сможете применить функцию управления сеансом, которая защищает конфиденциальные данные вашей организации от хищения и несанкционированного доступа в режиме реального времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


