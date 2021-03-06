---
title: Общие сведения об API Поиска новостей Bing
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать API Bing для поиска актуальных новостей в Интернете, включая заголовки в нескольких категориях и популярные темы.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: c6fad3a006d2f3da74638e0680d6ba65f736ab7b
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351237"
---
# <a name="what-is-the-bing-news-search-api"></a>Общие сведения об API Поиска новостей Bing

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

API Bing для поиска новостей позволяет легко интегрировать возможности когнитивного поиска новости Bing в приложениях. Интерфейс API предоставляет аналогичное взаимодействие для [новостей Bing](https://www.bing.com/news), позволяя отправлять поисковые запросы и получать соответствующие статьи.

Имейте в виду, что API Bing для поиска новостей предоставляет только результаты поиска новостей. Используйте [API Bing для поиска в Интернете](../bing-web-search/overview.md), [API для поиска видео](../bing-video-search/overview.md) и [API Bing для поиска изображений](../bing-image-search/overview.md) для других типов веб-содержимого.

## <a name="bing-news-search-api-features"></a>Возможности API Bing для поиска новостей

API Bing для поиска новостей в основном находит и возвращает соответствующие статьи, а также предоставляет несколько функций для интеллектуального и тематического поиска новостей в Интернете.

|Компонент  |Описание  |
|---------|---------|
|[Предложение и использование условий поиска](concepts/search-for-news.md#suggest-and-use-search-terms)     | Улучшите возможности поиска, используя [API автозаполнения Bing](../bing-autosuggest/get-suggested-search-terms.md) для отображения предлагаемых условий поиска по мере их ввода.         |
|[Получение общих новостей](concepts/search-for-news.md#get-general-news)     | Ищите новости, отправляя поисковый запрос в API Bing для поиска новостей и получая список соответствующих новостных статей.           |
|[Главные новости за сегодня](concepts/search-for-news.md#get-todays-top-news)      | Получите главные новости дня по всем категориям.       |
|[Новости по категории](concepts/search-for-news.md)     | Ищите новости в определенной категории.        | 
|[Сводка новостей](concepts/search-for-news.md)     | Ищите главные заголовки по всем категориям.         |

## <a name="workflow"></a>Рабочий процесс

API Bing для поиска новостей является веб-службой RESTful, которую легко вызвать с любого языка программирования, поддерживающего выполнение HTTP-запросов и анализа JSON. Вы можете использовать службу с помощью REST API или пакета SDK.

1. Создайте [учетную запись API Cognitive Services](../cognitive-services-apis-create-account.md) с доступом к API-интерфейсам поиска Bing. Если у вас нет подписки Azure, создайте бесплатную [учетную запись](https://azure.microsoft.com/free/cognitive-services/).
2. Отправьте запрос к API с допустимым поисковым запросом.
3. Обработайте ответ API путем анализа возвращенного сообщения JSON.

## <a name="next-steps"></a>Дальнейшие действия

Сначала попробуйте [интерактивную демоверсию](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) API Bing для поиска новостей. В этой демоверсии показано, как можно быстро настроить поисковый запрос и найти новости в Интернете.

Чтобы быстро приступить к работе с первым запросом API, изучите статью [Краткое руководство. Поиск новостей с помощью C# и REST API Bing для поиска новостей](./csharp.md) или [Краткое руководство. Поиск новостей с помощью пакета SDK Поиска новостей Bing для C#](./quickstarts/client-libraries.md?pivots=programming-language-csharp).

## <a name="see-also"></a>См. также раздел

* В [этой статье](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) содержатся определения и сведения о конечных точках, заголовках, ответах API и параметрах запроса, которые вы можете использовать для запроса результатов поиска на основе изображения.
* В статье [Требования к использованию и отображению API поиска Bing](../bing-web-search/use-display-requirements.md) рассматриваются приемлемые варианты использования содержимого и информации, получаемой с помощью API Bing для поиска.
* Сведения о других доступных API "Поиск Bing" см. на [главной странице](../bing-web-search/overview.md).