---
title: Когнитивный навык извлечения ключевой фразы
titleSuffix: Azure Cognitive Search
description: Оценивает неструктурированный текст и для каждой записи возвращает список ключевых фраз в конвейере обогащения с помощью ИИ в Когнитивном поиске Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 145d8265412c4efcaa2afad97feb23528a36191d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88936067"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Когнитивный навык извлечения ключевой фразы

Навык **Извлечение ключевой фразы** оценивает неструктурированный текст и для каждой записи возвращает список ключевых фраз. Этот навык использует модели машинного обучения, предоставляемые функцией [Анализ текста](../cognitive-services/text-analytics/overview.md) в Cognitive Services.

Эта возможность полезна, если необходимо быстро определить основные тезисы в записи. Например, для данного входного текста "Еда была вкусной и были замечательные сотрудники", служба вернет "еда" и "замечательные сотрудники".

> [!NOTE]
> По мере расширения области путем увеличения частоты обработки и добавления большего количества документов или дополнительных алгоритмов ИИ, вам нужно будет [присоединить оплачиваемый ресурс Cognitive Services](cognitive-search-attach-cognitive-services.md). Плата взимается при вызове API в Cognitive Services и извлечении изображений при распознавании документов в службе "Когнитивный поиск Azure". За извлечение текста из документов плата не взимается.
>
> Плата за выполнение встроенных навыков взимается в рамках существующей [модели оплаты Cognitive Services по мере использования](https://azure.microsoft.com/pricing/details/cognitive-services/). Плата за извлечение изображений указана на [странице с ценами на службу "Когнитивный поиск Azure"](https://azure.microsoft.com/pricing/details/search/).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Ограничения данных
Максимальный размер записи — 50 000 знаков, как определено в [`String.Length`](/dotnet/api/system.string.length). Если вам нужно разбить данные перед отправкой для извлечения ключевой фразы, можно воспользоваться [навыком разделения текста](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Параметры навыков

Параметры зависят от регистра.

| Входные данные                | Описание |
|---------------------|-------------|
| `defaultLanguageCode` | (Необязательно.) Код языка применяется к документам, в которых не указан язык явным образом.  Если код языка по умолчанию не указан, английский (en) используется как язык по умолчанию. <br/> Ознакомьтесь с [полным списком поддерживаемых языков](../cognitive-services/text-analytics/language-support.md). |
| `maxKeyPhraseCount`   | (Необязательно.) Максимальное количество ключевых фраз для создания. |

## <a name="skill-inputs"></a>Входные данные навыков

| Входные данные  | Описание |
|--------------------|-------------|
| `text` | Анализируемый текст.|
| `languageCode`    |  Строка, указывающая язык записей. Если этот параметр не указан, для анализа записей будет использоваться код языка по умолчанию. <br/>Ознакомьтесь с [полным списком поддерживаемых языков](../cognitive-services/text-analytics/language-support.md).|

## <a name="skill-outputs"></a>Выходные данные навыка

| Выходные данные     | Описание |
|--------------------|-------------|
| `keyPhrases` | Список ключевых фраз, извлеченных из вводимого текста. Ключевые фразы возвращаются в порядке важности. |


##  <a name="sample-definition"></a>Пример определения

Рассмотрим запись SQL, которая содержит следующие поля:

```json
{
    "content": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
    "language": "en"
}
```

Затем определение навыка может выглядеть следующим образом:

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      },
      {
        "name": "languageCode",
        "source": "/document/language" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
      }
    ]
  }
```

##  <a name="sample-output"></a>Пример полученных результатов

В приведенном выше примере выходные данные вашего навыка будут записаны в новый узел в расширенном дереве с именем Document/Микэйфрасес, поскольку именно `targetName` мы указали. Если не указать, будет `targetName` использоваться значение Document/фраз.

#### <a name="documentmykeyphrases"></a>Document/Микэйфрасес 
```json
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
```

Вы можете использовать "Document/Микэйфрасес" в качестве входных данных для других навыков или как источник [сопоставления полей вывода](cognitive-search-output-field-mapping.md).

## <a name="errors-and-warnings"></a>Ошибки и предупреждения
Если указать неподдерживаемый код языка, сформируется сообщение об ошибке, а ключевые фразы не будут извлечены.
Если текст пуст, появится предупреждение.
Если текст больше 50 000 знаков, будут проанализированы только первые 50 000 знаков и появится предупреждение.

## <a name="see-also"></a>См. также раздел

+ [Встроенные навыки](cognitive-search-predefined-skills.md)
+ [Определение набора навыков](cognitive-search-defining-skillset.md)
+ [Определение сопоставлений полей выходных данных](cognitive-search-output-field-mapping.md)