---
title: Настройка индексатора больших двоичных объектов
titleSuffix: Azure Cognitive Search
description: Настройка индексатора больших двоичных объектов Azure для автоматизации индексирования содержимого BLOB-объектов для операций полнотекстового поиска в Azure Когнитивный поиск.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: e3419711c9a7358914f85574f6dbd5af29def1cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91403622"
---
# <a name="how-to-configure-a-blob-indexer-in-azure-cognitive-search"></a>Как настроить индексатор BLOB-объектов в Azure Когнитивный поиск

В этой статье показано, как использовать Когнитивный поиск Azure для индексирования текстовых документов (например, документов PDF, документов Microsoft Office и некоторых других стандартных форматов), хранящихся в хранилище BLOB-объектов Azure. Во-первых, объясняются основные принципы установки и настройки индексатора больших двоичных объектов. Затем предлагается более углубленно изучить возможные сценарии и поведения.

<a name="SupportedFormats"></a>

## <a name="supported-formats"></a>Поддерживаемые форматы

Индексатор больших двоичных объектов может извлечь текст из документов следующих форматов:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="set-up-blob-indexing"></a>Настройка индексирования BLOB-объектов

Можно настроить индексатор хранилище BLOB-объектов Azure с помощью следующих инструментов:

* [Портал Azure](https://ms.portal.azure.com)
* [REST API](/rest/api/searchservice/Indexer-operations) когнитивный Поиск Azure
* [Пакет SDK](/dotnet/api/overview/azure/search) Azure когнитивный Поиск для .NET

> [!NOTE]
> Некоторые функции (например, сопоставление полей) еще не доступны на портале и должны быть использованы посредством кода.
>

Здесь демонстрируется поток с использованием интерфейса REST API.

### <a name="step-1-create-a-data-source"></a>Шаг 1. Создание источника данных

Источник данных определяет, какие данные следует индексировать, какие учетные данные требуются для доступа к данным и какие политики нужны, чтобы эффективно выявлять изменения в данных (новые, измененные или удаленные строки). Источник данных могут использовать несколько индексаторов в одной службе поиска.

Чтобы индексировать большие двоичные объекты, источник данных должен иметь следующие необходимые свойства.

* Свойство **name** — уникальное имя источника данных в службе поиска.
* Свойство **type** должно иметь значение `azureblob`.
* * * учетные данные предоставляют строку подключения учетной записи хранения в качестве `credentials.connectionString` параметра. Дополнительные сведения см. в разделе [Как указать учетные данные](#Credentials) ниже.
* Свойство **container** определяет контейнер в вашей учетной записи хранения. По умолчанию все большие двоичные объекты в контейнере доступны для извлечения. Если требуется индексирование больших двоичных объектов из определенного виртуального каталога, можно указать этот каталог с помощью дополнительного параметра **query**.

Создание источника данных

```http
    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }
```

Чтобы больше узнать об API создания источника данных, ознакомьтесь с [созданием источника данных](/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>

#### <a name="how-to-specify-credentials"></a>Как указать учетные данные

Учетные данные для контейнера BLOB-объектов можно указать одним из описанных ниже способов.

* **Строка подключения управляемого удостоверения**: `ResourceId=/subscriptions/<your subscription ID>/resourceGroups/<your resource group name>/providers/Microsoft.Storage/storageAccounts/<your storage account name>/;` 

  Для этой строки подключения не требуется ключ учетной записи, но необходимо выполнить инструкции по [настройке подключения к учетной записи хранения Azure с помощью управляемого удостоверения](search-howto-managed-identities-storage.md).

* **Строка подключения учетной записи хранения с полным доступом**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`

  Строку подключения можно получить на портале Azure, перейдя в колонку учетной записи хранения и щелкнув "Параметры" > "Ключи" (для классических учетных записей хранения) или "Параметры" > "Ключи доступа" (для учетных записей хранения Azure Resource Manager).

* Строка подключения для **подписи общего доступа к учетной записи хранения** (SAS):`BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`

  Подписанный URL-адрес должен иметь разрешения "Список" и "Чтение" для контейнеров и объектов (в данном случае — больших двоичных объектов).

* **Подписанный URL для общего доступа**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`

  Подписанный URL-адрес должен иметь разрешения "Список" и "Чтение" для контейнера.

Дополнительные сведения о подписанных URL для общего доступа см. в разделе [использование подписанных](../storage/common/storage-sas-overview.md)URL.

> [!NOTE]
> Если используются учетные данные SAS, то необходимо периодически обновлять учетные данные источника данных с помощью обновленных подписей, чтобы не истек их срок действия. В случае истечения срока действия учетных данных SAS произойдет сбой индексатора и появится сообщение об ошибке примерно следующего содержания: `Credentials provided in the connection string are invalid or have expired.`.  

### <a name="step-2-create-an-index"></a>Шаг 2. Создание индекса

Индекс задает поля в документе, атрибуты, и другие компоненты, которые определяют процедуру поиска.

Ниже описан процесс создания индекса с доступным для поиска полем `content` для хранения текста, извлеченного из больших двоичных объектов:

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

Дополнительные сведения см. в разделе [Создание индекса (REST API)](/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>Шаг 3. Создание индексатора

Индексатор соединяет источник данных с целевым индексом поиска и предоставляет расписание для автоматизации обновления данных.

Когда индекс и источник данных уже созданы, можно создать индексатор:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Этот индексатор будет выполняться каждые два часа (интервал расписания имеет значение "PT2H"). Чтобы запускать индексатор каждые 30 минут, задайте интервал "PT30M". Самый короткий интервал, который можно задать, составляет 5 минут. Расписание является необязательным. Если оно не указано, то индексатор выполняется только один раз при его создании. Однако индексатор можно запустить по запросу в любое время.   

Дополнительные сведения см. в разделе [CREATE индексатор (REST API)](/rest/api/searchservice/create-indexer). Дополнительные сведения об определении расписаний индексаторов для Когнитивного поиска Azure см. [здесь](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-blobs-are-indexed"></a>Индексирование больших двоичных объектов

В зависимости от [конфигурации](#PartsOfBlobToIndex) индексатор больших двоичных объектов может индексировать только метаданные хранилища (удобно, если вам необходимо индексировать только метаданные и не требуется индексировать содержимое больших двоичных объектов), метаданные хранилища и содержимое или и метаданные, и текстовое содержимое. По умолчанию индексатор извлекает и метаданные, и содержимое.

> [!NOTE]
> По умолчанию большие двоичные объекты со структурированным содержимым, например JSON- или CSV-файлы, индексируются как один блок текста. Чтобы получить дополнительные сведения об индексировании больших двоичных объектов JSON и CSV, см. раздел [индексирование больших](search-howto-index-json-blobs.md) двоичных объектов JSON и [индексирование больших двоичных объектов CSV](search-howto-index-csv-blobs.md) .
>
> Составной или внедренный документ (например, ZIP-архив, документ Word с внедренным электронным сообщением Outlook, содержащий вложения) или. MSG-файл с вложениями) также индексируется как один документ. Например, все изображения, извлеченные из вложений. Файл MSG будет возвращен в поле normalized_images.

* Текстовое содержимое документа извлекается в строковое поле `content`.

  > [!NOTE]
  > Azure Когнитивный поиск ограничивает объем извлекаемого текста в зависимости от ценовой категории: 32 000 символов для уровня Free, 64 000 для Basic, 4 000 000 для Standard, 8 000 000 для Standard S2 и 16 000 000 для уровня Standard S3. Предупреждение об усеченных документах отобразится в возвращенном состоянии индексатора.  

* В большом двоичном объекте определяемые пользователем свойства метаданных извлекаются без изменений. Обратите внимание, что для этого требуется, чтобы поле было определено в индексе с тем же именем, что и ключ метаданных большого двоичного объекта. Например, если у большого двоичного объекта есть ключ метаданных `Sensitivity` со значением `High` , необходимо определить поле с именем `Sensitivity` в индексе поиска, которое будет заполнено значением `High` .

* Стандартные свойства метаданных большого двоичного объекта извлекаются в указанные ниже поля.

  * **metadata\_storage\_name** (Edm.String) — имя файла большого двоичного объекта. Например, для большого двоичного объекта /my-container/my-folder/subfolder/resume.pdf значение этого поля — `resume.pdf`.

  * **metadata\_storage\_path** (Edm.String) — полный универсальный код ресурса (URI) большого двоичного объекта, включая учетную запись хранения. Например `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`.

  * **metadata\_storage\_content\_type** (Edm.String) — тип содержимого, указанный в коде для отправки большого двоичного объекта. Например, `application/octet-stream`.

  * **metadata\_storage\_last\_modified** (Edm.DateTimeOffset) — последняя измененная метка времени для большого двоичного объекта. Когнитивный поиск Azure использует эту метку времени для обнаружения измененных больших двоичных объектов, чтобы избежать повторного индексирования всех объектов после первоначального индексирования.

  * **metadata\_storage\_size** (Edm.Int64) — размер большого двоичного объекта в байтах.

  * **metadata\_storage\_content\_metadatastoragecontentmd5** (Edm.String) — хэш MD5 содержимого большого двоичного объекта, если он доступен.

  * **metadata\_storage\_sas\_token** (Edm.String) — временный маркер SAS, который может использоваться [пользовательскими навыками](cognitive-search-custom-skill-interface.md) для получения доступа к большому двоичному объекту. Этот токен не должен храниться для дальнейшего использования, так как срок его действия может истечь.

* Определенные для каждого формата документа свойства метаданных извлекаются в поля, список которых приводится [здесь](#ContentSpecificMetadata).

Вам не нужно определять поля для всех перечисленных свойств в индексе поиска — достаточно записать свойства, необходимые для приложения.

> [!NOTE]
> Как правило, имена полей в существующем индексе отличаются от имен полей, созданных при извлечении документа. **Сопоставления полей** можно использовать для сопоставления имен свойств, предоставленных когнитивный Поиск Azure, с именами полей в индексе поиска. Ниже вы увидите пример использования сопоставления полей.
>

<a name="DocumentKeys"></a>

### <a name="defining-document-keys-and-field-mappings"></a>Определение ключей документа и сопоставлений полей

В Azure Когнитивный поиск ключ документа однозначно определяет документ. Каждый индекс поиска должен содержать только одно поле ключа типа Edm.String. Поле ключа является обязательным для каждого документа, который добавляется к индексу (собственно, это единственное обязательное для заполнения поле).  

Следует определить, какие извлеченные поля должны сопоставляться с полем ключа индекса. Основные варианты представлены ниже.

* **metadata\_storage\_name** — удобный вариант, но следует учесть следующее. 1) Имена могут не быть уникальными, так как возможно наличие больших двоичных объектов с одинаковыми именами в разных папках. 2) Имя может содержать знаки, недопустимые для использования в ключах документов, например дефисы. Устранить проблему недопустимых знаков можно с помощью  [функции сопоставления полей](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`. После ее применения обязательно закодируйте ключи документов для передачи в вызовах API, таких как вызов для поиска. (Например, в .NET для этого можно использовать [метод UrlTokenEncode](/dotnet/api/system.web.httpserverutility.urltokenencode)).

* **metadata\_storage\_path** — использование полного пути гарантирует уникальность, но такой путь определенно содержит знаки `/`, [недопустимые в ключе документа](/rest/api/searchservice/naming-rules).  Как упоминалось выше, вы можете закодировать ключи с помощью  [функции](search-indexer-field-mappings.md#base64EncodeFunction)`base64Encode`.

* Третьим вариантом является добавление пользовательского свойства метаданных в большие двоичные объекты. Однако этот параметр требует, чтобы процесс передачи больших двоичных объектов добавил это свойство метаданных ко всем большим двоичным объектам. Поскольку ключ является обязательным свойством, все большие двоичные объекты без этого свойства индексироваться не будут.

> [!IMPORTANT]
> Если для ключевого поля в индексе нет явного сопоставления, Azure Когнитивный поиск автоматически использует `metadata_storage_path` в качестве ключа, а Base-64 кодирует значения ключей (второй параметр выше).
>
>

В этом примере мы выберем поле `metadata_storage_name` в качестве ключа документа. Предположим, что индекс содержит поле ключа с именем `key` и поле `fileSize` для хранения данных о размере документа. Чтобы правильно связать все компоненты при создании или обновлении индексатора, укажите следующие сопоставления полей:

```http
    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]
```

Чтобы объединить все компоненты, можно добавить сопоставления полей и активировать кодировку ключей base-64 для существующего индексатора:

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }
```

Дополнительные сведения см. в разделе [сопоставления полей и преобразования](search-indexer-field-mappings.md).

#### <a name="what-if-you-need-to-encode-a-field-to-use-it-as-a-key-but-you-also-want-to-search-it"></a>Что делать, если необходимо закодировать поле для использования в качестве ключа, но вы также хотите выполнить его поиск?

Иногда требуется использовать закодированную версию поля, например metadata_storage_path в качестве ключа, но также необходимо, чтобы поле было доступно для поиска (без кодирования). Чтобы устранить эту проблему, можно сопоставить ее с двумя полями. один, который будет использоваться для ключа, и другой, который будет использоваться в целях поиска. В примере ниже *ключевое* поле содержит закодированный путь, а поле *пути* не кодируется и будет использоваться в качестве поля для поиска в индексе.

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "path" }
      ]
    }
```
<a name="WhichBlobsAreIndexed"></a>

## <a name="index-by-file-type"></a>Индекс по типу файла

Вы можете выбирать, какие большие двоичные объекты необходимо индексировать, а какие — пропустить.

### <a name="include-blobs-having-specific-file-extensions"></a>Включить большие двоичные объекты с особыми расширениями файлов

Параметр конфигурации индексатора `indexedFileNameExtensions` позволяет индексировать большие двоичные объекты только с указанными расширениями имен файлов. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, чтобы индексировать только большие двоичные объекты PDF и DOCX, выполните следующую команду:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }
```

### <a name="exclude-blobs-having-specific-file-extensions"></a>Исключить BLOB-объекты с определенными расширениями файлов

Большие двоичные объекты с определенными расширениями файлов можно исключить из индексации с помощью параметра конфигурации `excludedFileNameExtensions`. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, для индексации всех больших двоичных объектов, кроме объектов с расширениями PNG и JPEG, выполните следующую команду:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }
```

Если `indexedFileNameExtensions` существуют оба `excludedFileNameExtensions` параметра и, Azure когнитивный поиск сначала просматривает `indexedFileNameExtensions` , а затем по адресу `excludedFileNameExtensions` . Это означает, что если одно расширение файла присутствует в обоих списках, объект будет исключен из индексации.

<a name="PartsOfBlobToIndex"></a>

## <a name="index-parts-of-a-blob"></a>Индексные части большого двоичного объекта

Вы можете выбрать, какие части большого двоичного объекта необходимо индексировать, с помощью параметра конфигурации `dataToExtract`. Этот параметр может принимать перечисленные ниже значения.

* `storageMetadata` — указывает, что необходимо индексировать только [стандартные свойства большого двоичного объекта и метаданные, определяемые пользователем](../storage/blobs/storage-blob-container-properties-metadata.md).
* `allMetadata` — указывает, что необходимо индексировать метаданные хранилища и [метаданные определенного типа содержимого](#ContentSpecificMetadata), извлеченные из содержимого большого двоичного объекта.
* `contentAndMetadata` — указывает, что необходимо индексировать все метаданные и текстовое содержимое, извлеченное из большого двоичного объекта. Это значение по умолчанию.

Например, чтобы индексировать только метаданные хранилища, используйте следующую команду:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }
```

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Использование метаданных большого двоичного объекта для определения способа индексирования больших двоичных объектов

Описанные выше параметры конфигурации применяются ко всем большим двоичным объектам. В некоторых случаях вам может потребоваться определять способ индексирования *отдельных больших двоичных объектов*. Это можно сделать, добавив следующие свойства и значения метаданных большого двоичного объекта.

| Имя свойства | Значение свойства | Объяснение |
| --- | --- | --- |
| AzureSearch_Skip |True |Указывает индексатору больших двоичных объектов пропустить весь большой двоичный объект. Не извлекаются ни метаданные, ни содержимое. Это полезно, когда обработка определенного большого двоичного объекта постоянно завершается сбоем и индексирование прерывается. |
| AzureSearch_SkipContent |True |Это эквивалент параметра `"dataToExtract" : "allMetadata"`, описанного [выше](#PartsOfBlobToIndex), относящегося к определенному большому двоичному объекту. |

## <a name="index-from-multiple-sources"></a>Индекс из нескольких источников

Возможно, вам нужно создать в индексе "сборку" документов из нескольких источников. Например, можно объединить текст из BLOB-объектов с другими метаданными, хранящимися в Cosmos DB. Можно даже использовать API принудительного индексирования вместе с другими индексаторами, чтобы создавать документы для поиска из нескольких частей.

Чтобы это работало, ключ документа должен быть согласован для всех индексаторов и других компонентов. Дополнительные сведения об этом разделе см. в статье [индексирование нескольких источников данных Azure](./tutorial-multiple-data-sources.md) или в этой записи блога. [Объедините документы с другими данными в когнитивный Поиск Azure](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

## <a name="index-large-datasets"></a>Индексирование больших наборов данных

Индексирование больших двоичных объектов может занимать много времени. Если вам необходимо индексировать миллионы больших двоичных объектов, этот процесс можно ускорить, секционировав данные и используя несколько индексаторов для обработки данных в параллельном режиме. Вот как это можно сделать.

* Секционируйте данные в несколько контейнеров больших двоичных объектов или виртуальных папок.

* Настройте несколько источников данных Azure Когнитивный поиск, по одному на контейнер или папку. Чтобы указать папку большого двоичного объекта, используйте параметр `query`:

    ```json
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

* Создайте соответствующий индексатор для каждого источника данных. Все индексаторы могут указывать на один и тот же целевой индекс поиска.  

* Одна единица поиска в службе в определенный момент времени может запустить один индексатор. Создание нескольких индексаторов, как описано выше, полезно только при их фактическом параллельном выполнении. Для параллельного выполнения нескольких индексаторов следует увеличить масштаб службы поиска, создав достаточное число секций и реплик. Например, если служба поиска содержит 6 единиц поиска (скажем, 2 секции x 3 реплики), то параллельно могут выполняться 6 индексаторов. Это позволяет достичь шестикратного увеличения пропускной способности индексации. Дополнительные сведения о масштабировании и планировании емкости см. в статье [Настройка емкости службы когнитивный Поиск Azure](search-capacity-planning.md).

<a name="DealingWithErrors"></a>

## <a name="handle-errors"></a>Обработка ошибок

По умолчанию индексатор больших двоичных объектов останавливается, как только обнаружит большой двоичный объект с неподдерживаемым типом содержимого (например, изображение). Чтобы пропустить определенные типы содержимого, можно воспользоваться параметром `excludedFileNameExtensions`. Тем не менее может потребоваться индексировать большие двоичные объекты, не зная все возможные типы их содержимого. Чтобы продолжить индексирование при обнаружении неподдерживаемого типа содержимого, задайте для параметра конфигурации `failOnUnsupportedContentType` значение `false`:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }
```

Для некоторых больших двоичных объектов Azure Когнитивный поиск не может определить тип содержимого или обработать документ из другого типа содержимого, поддерживаемого другим способом. Чтобы пропустить этот сбой, задайте для `failOnUnprocessableDocument` параметра конфигурации значение False:

```http
      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }
```

Azure Когнитивный поиск ограничивает размер индексируемых больших двоичных объектов. Эти ограничения описаны в статье [ограничения службы в Azure когнитивный Поиск](./search-limits-quotas-capacity.md). Большие двоичные объекты слишком большого размера по умолчанию считаются ошибками. Но вы все равно можете индексировать метаданные хранилища больших двоичных объектов слишком большого размера, если для параметра конфигурации `indexStorageMetadataOnlyForOversizedDocuments` задать значение true:

```http
    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }
```

Вы также можете продолжить индексирование, если ошибки возникают в какой-либо момент обработки — при анализе больших двоичных объектов или при добавлении документов в индекс. Чтобы пропустить определенное количество ошибок, задайте для параметров конфигурации `maxFailedItems` и `maxFailedItemsPerBatch` нужные значения. Пример:

```http
    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }
```

<a name="ContentSpecificMetadata"></a>

## <a name="content-type-specific-metadata-properties"></a>Свойства метаданных определенного типа содержимого

В следующей таблице приводится сводка по обработке для каждого формата документа и описываются свойства метаданных, извлеченные Когнитивный поиск Azure.

| Формат документа или тип содержимого | Извлеченные метаданные | Сведения об обработке |
| --- | --- | --- |
| HTML (text/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |Удаление разметки HTML и извлечение текста |
| PDF (приложение/PDF) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Извлечение текста, включая внедренные документы (кроме изображений) |
| DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| DOC (application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| DOCM (Application/vnd.ms-word.docумент. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| WORD XML (application/vnd. MS-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Удаление разметки XML и извлечение текста |
| WORD 2003 XML (application/vnd. МС-вордмл) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |Удаление разметки XML и извлечение текста |
| XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| XLS (application/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| XLSM (приложение/vnd. MS-Excel а. sheet. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| PPT (application/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| PPTM (приложение/vnd. МС-поверпоинт. Presentation. макроенаблед. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Извлечение текста, включая внедренные документы |
| MSG (application/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Извлеките текст, включая текст, извлеченный из вложений. `metadata_message_to_email``metadata_message_cc_email`и `metadata_message_bcc_email` являются строковыми коллекциями, остальные поля являются строками.|
| ODT (application/vnd. Oasis. OpenDocument. Text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Извлечение текста, включая внедренные документы |
| ODS (application/vnd. Oasis. OpenDocument. электронных таблиц) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Извлечение текста, включая внедренные документы |
| ODP (приложение/vnd. Oasis. OpenDocument. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Извлечение текста, включая внедренные документы |
| ZIP (application/zip) |`metadata_content_type` |Извлечение текста из всех документов в архиве |
| GZ (приложение/gzip) |`metadata_content_type` |Извлечение текста из всех документов в архиве |
| ЕПУБ (Application/епуб + ZIP) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Извлечение текста из всех документов в архиве |
| XML (application/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |Удаление разметки XML и извлечение текста |
| JSON (application/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Извлечение текста<br/>Примечание. Инструкции по извлечению нескольких полей документа из большого двоичного объекта JSON см. в статье [Индексирование BLOB-объектов JSON с помощью индексатора BLOB-объектов службы поиска Azure](search-howto-index-json-blobs.md). |
| EML (message/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Извлечение текста, включая вложения |
| RTF (приложение или RTF) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Извлечение текста|
| Обычный текст (text/plain) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Извлечение текста|

## <a name="see-also"></a>См. также

* [Indexers in Azure Cognitive Search](search-indexer-overview.md) (Индексаторы в службе "Когнитивный поиск Azure")
* [Общие сведения о больших двоичных объектах с помощью AI](search-blob-ai-integration.md)
* [Общие сведения об индексировании BLOB-объектов](search-blob-storage-integration.md)