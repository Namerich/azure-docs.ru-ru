---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: a3e32dba557041fe90d1b7321b06daa6ea4e1235
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586765"
---
[!INCLUDE [Prerequisites](prerequisites-python.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Python, используя любую IDE или любой текстовый редактор. Затем скопируйте в файл проекта с именем `sentence-length.py` этот фрагмент кода.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Если вы ранее не использовали эти модули, вам потребуется установить их перед запуском программы. Чтобы установить эти пакеты, выполните команду: `pip install requests uuid`.

Первый комментарий сообщает вашему интерпретатору Python об использовании кодировки UTF-8. Затем требуемые модули импортируются для считывания ключа подписки из переменной среды, создания запроса HTTP, создания уникального идентификатора и обработки ответа JSON, возвращаемого Переводчиком.

## <a name="set-the-subscription-key-endpoint-and-path"></a>Выбор ключа подписки, конечной точки и пути

В этом примере будет предпринята попытка считать ключ подписки Переводчика из переменных среды `TRANSLATOR_TEXT_KEY` и `TRANSLATOR_TEXT_ENDPOINT`. Если вы не знакомы с переменными среды, можно задать `subscription_key` и `endpoint` в виде строк и закомментировать условные операторы.

Скопируйте в проект следующий код:

```python
key_var_name = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY'
if not key_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(key_var_name))
subscription_key = os.environ[key_var_name]

endpoint_var_name = 'TRANSLATOR_TEXT_ENDPOINT'
if not endpoint_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(endpoint_var_name))
endpoint = os.environ[endpoint_var_name]
```

Конечная точка Переводчика задается как `endpoint`. Параметр `path` задает маршрут `breaksentence` и определяет, что нужно использовать версию 3 API.

`params` в этом примере используются для задания языка указанного текста. `params` не являются обязательными для маршрута `breaksentence`. Если их исключить из запроса, API попытается определить язык заданного текста и предоставит эти сведения в ответе вместе с оценкой достоверности.

>[!NOTE]
> Дополнительные сведения о конечных точках, маршрутах и параметрах запросов см. в руководстве по [Переводчику 3.0. Languages](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

```python
path = '/breaksentence?api-version=3.0'
params = '&language=en'
constructed_url = endpoint + path + params
```

## <a name="add-headers"></a>Добавление заголовков

Самый простой способ выполнить проверку подлинности запроса — это передать ключ подписки как заголовок `Ocp-Apim-Subscription-Key`, который мы используем в этом примере. В качестве альтернативы вы можете обменять свой ключ подписки на маркер доступа и передать маркер доступа в виде заголовка `Authorization` для проверки своего запроса. Дополнительные сведения см. в разделе [Authenticate to the Speech API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) (Аутентификация в API речи).

Скопируйте в проект следующий фрагмент кода:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

Если вы используете подписку на несколько служб Cognitive Services, необходимо также включить `Ocp-Apim-Subscription-Region` в параметрах запроса. [Дополнительные сведения об аутентификации с использованием подписки на несколько служб](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-request-to-determine-sentence-length"></a>Создание запроса для определения длины предложения

Определите предложения, длину которых нужно выяснить.

```python
# You can pass more than one object in body.
body = [{
    'text': 'How are you? I am fine. What did you do today?'
}]
```

Далее мы создадим запрос POST, используя модуль `requests`. Требуется три аргумента: сцепленный URL-адрес, заголовки запроса и текст запроса:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Вывод ответа

Последний шаг — вывести результаты. Этот фрагмент кода упорядочивает результаты, сортируя ключи, устанавливая отступы и объявляя разделители элементов и ключей.

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Сборка

Вот и все. Вы собрали простую программу, которая вызовет Переводчика и вернет ответ в формате JSON. Теперь пришло время запустить ее.

```console
python sentence-length.py
```

Если вы хотите сравнить свой код с нашей версией, см. [пример кода на сайте GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Пример ответа

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы закодировали свой ключ подписки в программе, обязательно удалите его после завершения работы с этим кратким руководством.

## <a name="next-steps"></a>Дальнейшие действия

Просмотрите справочник по API, чтобы получить представление обо всех возможностях Переводчика.

> [!div class="nextstepaction"]
> [Справочник по API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
