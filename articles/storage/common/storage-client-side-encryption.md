---
title: Шифрование на стороне клиента с помощью .NET для службы хранилища Microsoft Azure | Документация Майкрософт
description: Клиентская библиотека хранилища Azure для .NET поддерживает шифрование на стороне клиента, а также интеграцию с хранилищем ключей Azure для обеспечения максимальной защиты ваших приложений службы хранилища Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 11/10/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-csharp
ms.openlocfilehash: 5f2d3ba12fa65beb7156e056c23e44b028cbb520
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445070"
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Шифрование на стороне клиента для службы хранилища Microsoft Azure
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Обзор
[Клиентская библиотека службы хранилища Azure для .NET](/dotnet/api/overview/azure/storage) поддерживает шифрование данных в клиентских приложениях перед отправкой в службу хранилища Azure и расшифровку данных при скачивании на клиент. Библиотека также поддерживает интеграцию с [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) для управления ключами учетной записи хранения.

Подробный учебник, в котором последовательно описывается процесс шифрования больших двоичных объектов на стороне клиента с помощью хранилища ключей Azure, приведен в разделе [Шифрование и расшифровка BLOB-объектов в хранилище Microsoft Azure с помощью хранилища ключей Azure](../blobs/storage-encrypt-decrypt-blobs-key-vault.md).

Сведения о шифровании на стороне клиента с помощью Java см. в статье [Шифрование на стороне клиента с помощью Java для службы хранилища Microsoft Azure](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Шифрование и расшифровка методом конвертов
Для шифрования и расшифровки используется конвертный метод.

### <a name="encryption-via-the-envelope-technique"></a>Шифрование конвертным методом
Шифрование конвертным методом выполняется следующим образом.

1. Клиентская библиотека хранилища Azure создает ключ шифрования содержимого (CEK), который является симметричным ключом для однократного использования.
2. Данные пользователя шифруются с помощью этого ключа CEK.
3. Ключ CEK, в свою очередь, шифруется с помощью ключа шифрования ключа KEK. KEK определяется идентификатором ключа и может быть парой асимметричных ключей или симметричным ключом. Им можно управлять локально, а также хранить его в хранилище ключей Azure.
   
    Сама клиентская библиотека хранилища не имеет доступа к ключу KEK. Библиотека вызывает алгоритм шифрования ключа, который обеспечивается хранилищем ключей. Пользователи могут при необходимости использовать настраиваемые поставщики для шифрования и расшифровки ключа.

4. Зашифрованные данные затем передаются в службу хранилища Azure. Зашифрованный ключ вместе с дополнительными метаданными шифрования хранится как метаданные (в большом двоичном объекте) или вставляется в зашифрованные данные (сообщения в очереди и табличные сущности).

### <a name="decryption-via-the-envelope-technique"></a>Расшифровка конвертным методом
Расшифровка конвертным методом выполняется следующим образом.

1. Клиентская библиотека предполагает, что пользователь управляет ключом шифрования ключа KEK локально или через хранилище ключей Azure. Пользователь может не знать, какой именно ключ использовался для шифрования. Вместо этого достаточно настроить и использовать сопоставитель ключей, который будет распознавать разные идентификаторы ключей.
2. Клиентская библиотека скачивает зашифрованные данные вместе с данными шифрования, которые хранятся в службе.
3. Зашифрованный ключ шифрования содержимого CEK расшифровывается с помощью ключа шифрования ключа KEK. Клиентская библиотека не имеет доступа к ключу KEK. Она просто вызывает пользовательский алгоритм или алгоритм расшифровки поставщика хранилища ключей.
4. Затем ключ шифрования содержимого CEK используется для расшифровки зашифрованных пользовательских данных.

## <a name="encryption-mechanism"></a>Механизм шифрования
Клиентская библиотека хранилища использует алгоритм [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) для шифрования данных пользователя. Говоря более конкретно, это режим [цепочки цифровых блоков или CBC](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) вместе с AES. Каждая служба работает по-разному, поэтому каждая служба рассматривается отдельно.

### <a name="blobs"></a>BLOB-объекты
Клиентская библиотека в настоящий момент полностью поддерживает только шифрование больших двоичных объектов. Что касается загрузок, то поддерживаются как полные, так и диапазонные загрузки.

Во время шифрования клиентская библиотека создает случайный вектор инициализации IV размером 16 байт, случайный ключ шифрования содержимого CEK размером 32 байта и выполняет конвертное шифрование данных большого двоичного объекта, используя полученную информацию. Затем зашифрованный ключ CEK и дополнительные метаданные шифрования сохраняются в службе как метаданные большого двоичного объекта вместе с зашифрованным большим двоичным объектом.

> [!WARNING]
> Если вы изменяете или загружаете собственные метаданные для большого двоичного объекта, нужно убедиться, что данные сохранены. Если загрузить новые метаданные без этих метаданных, зашифрованный ключ CEK, ключ IV и другие метаданные будут утеряны, а без них получить содержимое большого двоичного объекта невозможно.
> 
> 

При скачивании всего большого двоичного объекта ключ CEK упаковывается и используется вместе с вектором инициализации (хранимые в этом случае как метаданные BLOB-объектов) для возврата расшифрованных данных пользователям.

Загрузка произвольного диапазона в зашифрованном большом двоичном объекте включает в себя настройку диапазона, предоставленного пользователями, чтобы получить небольшой объем дополнительных данных, которые можно использовать для успешной расшифровки запрошенного диапазона.

Все типы больших двоичных объектов (блочные, страничные и инкрементируемые) могут быть зашифрованы и расшифрованы с помощью этой схемы.

### <a name="queues"></a>Очереди
Поскольку очередь сообщений может иметь любой формат, клиентская библиотека определяет нестандартный формат, который включает вектор инициализации IV и зашифрованный ключ шифрования содержимого CEK в текст сообщения.

Во время шифрования клиентская библиотека создает случайный ключ IV размером 16 байт, случайный ключ CEK размером 32 байта и выполняет конвертное шифрование текста сообщения очереди, используя полученную информацию. Зашифрованный ключ CEK и дополнительные метаданные шифрования добавляются в зашифрованное сообщение очереди. Это измененное сообщение (показано ниже) сохраняется в службе.

```xml
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Во время расшифровки зашифрованный ключ извлекается из сообщения очереди и расшифровывается. Ключ IV также извлекается из сообщения очереди и используется вместе с расшифрованным ключом для расшифровки данных сообщения очереди. Обратите внимание, что размер метаданных шифрования очень мал (не более 500 байт), поэтому, хотя этот их размер учитывается при подсчете максимального размера в 64 КБ для сообщения очереди, этим размером данных можно пренебречь. Обратите внимание, что зашифрованное сообщение будет закодировано в кодировке Base64, как показано в приведенном выше фрагменте, который также расширяет размер отправляемого сообщения.

### <a name="tables"></a>Таблицы
> [!NOTE]
> Служба таблиц поддерживается в клиентской библиотеке службы хранилища Azure только по версии 9. x.
> 
> 

Клиентская библиотека поддерживает шифрование свойств сущности для операций вставки и замены.

> [!NOTE]
> Слияние в настоящее время не поддерживается. Поскольку подмножество свойств могло уже быть зашифровано с помощью другого ключа, простое слияние новых свойств и обновление метаданных приведет к потере данных. Для слияния требуется либо сначала прочитать данные существующей сущности в службе, либо использовать новый ключ для каждого свойства, однако оба способа не подходят из-за низкой эффективности.
> 
> 

Шифрование табличных данных выполняется следующим образом.  

1. Пользователи указывают свойства, которые следует зашифровать.
2. Клиентская библиотека создает случайный вектор инициализации IV размером 16 байт и произвольный ключ шифрования содержимого CEK размером 32 байта для каждой сущности и выполняет конвертное шифрование для отдельных свойств, которые следует зашифровать путем создания нового ключа IV для каждого свойства. Зашифрованное свойство хранится в виде двоичных данных.
3. Затем зашифрованный ключ CEK и дополнительные метаданные шифрования сохраняются в виде двух дополнительных зарезервированных свойств. Первое зарезервированное свойство (_ClientEncryptionMetadata1) — это строковое свойство, которое содержит информацию о ключе IV, версию и зашифрованный ключ. Второе зарезервированное свойство (_ClientEncryptionMetadata2) — это двоичное свойство, которое содержит информацию о зашифрованных свойствах. Сведения в этом втором свойстве (_ClientEncryptionMetadata2) сами по себе зашифрованы.
4. Из-за этих двух дополнительных зарезервированных свойств, необходимых для шифрования, пользователи могут иметь только 250 пользовательских свойств вместо 252. Общий размер сущности должен быть меньше 1 МБ.

Обратите внимание, что зашифрованы могут быть только строковые свойства. Если необходимо зашифровать другие типы свойств, их необходимо преобразовать в строки. Зашифрованные строки хранятся в службе в виде двоичных свойств. Они преобразуются обратно в строки после расшифровки.

Что касается таблиц, то в дополнение к политике шифрования пользователи должны указать свойства, которые необходимо зашифровать. Это можно сделать путем указания атрибута [PropertyAttribute] \(для сущностей POCO, которые являются производными от TableEntity) или с помощью сопоставителя шифрования в параметрах запроса. Сопоставитель шифрования — это делегат, который получает ключ секции, ключ строки и имя свойства, а затем возвращает логическое значение, которое указывает, следует ли это свойство шифровать. Во время шифрования клиентская библиотека использует эти сведения, чтобы решить, следует ли шифровать свойство перед отправкой. Делегат также обеспечивает возможность логики в отношении того, как шифруются свойства. (Например, если X, то зашифруйте свойство A; в противном случае зашифруйте свойства A и B.) Обратите внимание, что при чтении или запросе сущностей не обязательно указывать эти сведения.

### <a name="batch-operations"></a>Пакетные операции
В пакетных операциях один ключ KEK используется для всех строк операции, поскольку клиентская библиотека допускает использование только одного объекта параметров (и, следовательно, одну политику или ключ KEK) на одну пакетную операцию. Однако клиентская библиотека создает новый случайный ключ IV и случайный ключ CEK на каждую строку в пакете. Пользователи могут также выбрать шифрование других свойств для каждой операции в пакете, определив это поведение в сопоставителе шифрования.

### <a name="queries"></a>Запросы
> [!NOTE]
> Так как сущности зашифрованы, нельзя запускать запросы с фильтрацией для них.  Такая попытка даст неверные результаты, потому что в службе зашифрованные данные будут сравниваться с незашифрованными.
> 
> 
> Чтобы выполнять операции запроса, следует указать сопоставитель ключа, который способен распознавать все ключи в результирующем наборе. Если сущность в результате запроса не может быть разрешена для поставщика, клиентская библиотека выдаст ошибку. Для запроса, который обращается к серверу, по умолчанию клиентская библиотека добавляет к выбранным столбцам специальные свойства метаданных шифрования (_ClientEncryptionMetadata1 и _ClientEncryptionMetadata2).

## <a name="azure-key-vault"></a>Azure Key Vault
Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. Хранилище ключей Azure позволяет шифровать ключи и секреты (например, ключи проверки подлинности, ключи учетных записей хранения, ключи шифрования данных, PFX-файлы и пароли), используя ключи, защищенные аппаратными модулями безопасности. Дополнительные сведения см. в статье [Что такое хранилище ключей Azure?](../../key-vault/general/overview.md)

Клиентская библиотека хранилища использует интерфейсы Key Vault в основной библиотеке, чтобы предоставить общую платформу в Azure для управления ключами. Пользователи могут использовать библиотеки Key Vault для всех дополнительных преимуществ, которые они предоставляют, например полезные функциональные возможности простых и безотносительно локальных и облачных поставщиков ключей, а также объединение и кэширование.

### <a name="interface-and-dependencies"></a>Интерфейс и зависимости

# <a name="net-v12"></a>[Платформа .NET версии 12](#tab/dotnet)

Существует два необходимых пакета для интеграции Key Vault:

* Azure. Core содержит `IKeyEncryptionKey` интерфейсы и `IKeyEncryptionKeyResolver` . Клиентская библиотека хранилища для .NET уже определяет ее как зависимость.
* Azure. Security. KeyVault. Keys (v4. x) содержит клиент Key Vault RESTFUL, а также криптографические клиенты, используемые при шифровании на стороне клиента.

# <a name="net-v11"></a>[Версии 11 .NET](#tab/dotnet11)

Существует три пакета хранилища ключей:

* Microsoft.Azure.KeyVault.Core содержит IKey и IKeyResolver. Это небольшой пакет без зависимостей. Клиентская библиотека хранилища для .NET определяет это как зависимость.
* Microsoft. Azure. KeyVault (v3. x) содержит клиент Key Vault RESTFUL.
* Microsoft. Azure. KeyVault. Extensions (v3. x) содержит код расширения, включающий реализации алгоритмов шифрования, а также RSAKey и SymmetricKey. Пакет зависит от пространств имен Core и KeyVault и обеспечивает функции, которые позволяют определить составной сопоставитель (когда пользователи используют несколько поставщиков ключей) и сопоставитель ключа кэширования. Клиентская библиотека хранилища не зависит напрямую от этого пакета, однако, если пользователь хочет использовать хранилище ключей Azure для хранения ключей или расширения хранилища ключей для работы с локальными и облачными поставщиками служб шифрования, потребуется этот пакет.

Дополнительные сведения об использовании Key Vault в версии 11 можно найти в [примерах кода шифрования версии 11](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

---

Хранилище ключей разработано для главных ключей, и ограничения регулирования, связанные с хранилищем ключей, учитывают это. При выполнении шифрования на стороне клиента с помощью хранилища ключей предпочтительный способ — использовать симметричные главные ключи, которые хранятся в качестве секретов в хранилище ключей, и кэшировать их локально. Рекомендуется придерживаться следующих правил.

1. Создайте секрет без подключения к сети и загрузите его в хранилище ключей.
2. Используйте базовый идентификатор секрета в качестве параметра для разрешения текущей версии секрета для шифрования и кэшируйте эту информацию локально. Используйте параметр CachingKeyResolver для кэширования; пользователи не должны реализовывать собственную логику кэширования.
3. Во время создания политики шифрования используйте кэширующий сопоставитель в качестве входных данных.

## <a name="best-practices"></a>Рекомендации
Шифрование поддерживается только клиентской библиотекой хранилища для .NET. Windows Phone и среда выполнения Windows в настоящее время не поддерживают шифрование.

> [!IMPORTANT]
> Учтите следующие важные моменты при использовании шифрования на стороне клиента.
> 
> * При чтении или записи в зашифрованном большом двоичном объекте используйте команды загрузки полного большого двоичного объекта и команды диапазонной либо полной выгрузки. Избегайте написания зашифрованного большого двоичного объекта с помощью операций протокола, таких как Put Block, Put Block List, Write Pages, Clear Pages или Append Block, иначе это может привести к повреждению зашифрованного большого двоичного объекта, что сделает его нечитабельным.
> * Для таблиц существуют аналогичные ограничения. Будьте внимательны и не обновляйте зашифрованные свойства без обновления метаданных шифрования.
> * Если задать метаданные в зашифрованном большом двоичном объекте, могут быть перезаписаны метаданные, относящиеся к шифрованию и необходимые для расшифровки, поскольку настройку метаданных добавить нельзя. Это также касается моментальных снимков. Не указывайте метаданные во время создания моментального снимка зашифрованного большого двоичного объекта. Если необходимо задать метаданные, следует сначала вызвать метод **FetchAttributes** для получения текущих метаданных шифрования и избегать параллельных операций записи во время установки метаданных.
> * Включите свойство **RequireEncryption** в параметры запроса по умолчанию для пользователей, которые должны работать только с зашифрованными данными. См. дополнительные сведения ниже.
>
>

## <a name="client-api--interface"></a>API-интерфейс клиента / интерфейс
Пользователи могут предоставлять только ключ, только сопоставитель или и то, и другое. Ключи идентифицируются с помощью идентификатора ключа и обеспечивают логику для упаковки и распаковки. Арбитры конфликтов используются для разрешения ключа в процессе расшифровки. Он определяет метод Resolve, возвращающий ключ по заданному идентификатору ключа. В результате пользователи могут выбирать между несколькими ключами, которые хранятся в разных местах.

* Для шифрования ключ используется всегда, при этом отсутствие ключа приведет к возникновению ошибки.
* Для расшифровки:
  * Если ключ указан и его идентификатор соответствует требуемому идентификатору ключа, этот ключ используется для расшифровки. В противном случае предпринимается ошибка распознавателя. Если для этой попытки не существует сопоставителя, выдается ошибка.
  * Сопоставитель ключа вызывается, если он нужен для получения ключа. Если сопоставитель указан, но он не имеет данных, сопоставимых с идентификатором ключа, возникает ошибка.

### <a name="requireencryption-mode-v11-only"></a>Режим RequireEncryption (только версии 11)
При необходимости можно включить режим работы, где все передачи и загрузки должны быть зашифрованы. В этом режиме все попытки клиента передать данные без политики шифрования или загрузить данные, которые не зашифрованы в службе, закончатся ошибкой. Это поведение контролирует свойство **RequireEncryption** объекта параметров запроса. Если приложение будет шифровать все объекты из службы хранилища Azure, то можно задать свойство **requireEncryption** в параметрах запроса по умолчанию для объекта клиента службы. Например, задайте для **CloudBlobClient.DefaultRequestOptions.RequireEncryption** значение **true** , чтобы требовать шифрования всех операций с большими двоичными объектами, выполненных посредством этого объекта клиента.


### <a name="blob-service-encryption"></a>Шифрование службы BLOB-объектов


# <a name="net-v12"></a>[Платформа .NET версии 12](#tab/dotnet)
Создайте объект **клиентсидинкриптионоптионс** и настройте его при создании клиента с помощью **спеЦиализедблобклиентоптионс**. Задать параметры шифрования для каждого API нельзя. Все остальные задачи решаются клиентской библиотекой.

```csharp
// Your key and key resolver instances, either through KeyVault SDK or an external implementation
IKeyEncryptionKey key;
IKeyEncryptionKeyResolver keyResolver;

// Create the encryption options to be used for upload and download.
ClientSideEncryptionOptions encryptionOptions = new ClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Set the encryption options on the client options
BlobClientOptions options = new SpecializedBlobClientOptions() { ClientSideEncryption = encryptionOptions };

// Get your blob client with client-side encryption enabled.
// Client-side encryption options are passed from service to container clients, and container to blob clients.
// Attempting to construct a BlockBlobClient, PageBlobClient, or AppendBlobClient from a BlobContainerClient
// with client-side encryption options present will throw, as this functionality is only supported with BlobClient.
BlobClient blob = new BlobServiceClient(connectionString, options).GetBlobContainerClient("myContainer").GetBlobClient("myBlob");

// Upload the encrypted contents to the blob.
blob.Upload(stream);

// Download and decrypt the encrypted contents from the blob.
MemoryStream outputStream = new MemoryStream();
blob.DownloadTo(outputStream);
```

**Используются службой blobserviceclient** не требуется применять параметры шифрования. Они также могут передаваться в конструкторы **блобконтаинерклиент** / **блобклиент** , которые принимают объекты **блобклиентоптионс** .

Если нужный объект **блобклиент** уже существует, но без параметров шифрования на стороне клиента, существует метод расширения для создания копии этого объекта с заданным **клиентсидинкриптионоптионс**. Этот метод расширения позволяет избежать издержек при создании нового объекта **блобклиент** с нуля.

```csharp
using Azure.Storage.Blobs.Specialized;

// Your existing BlobClient instance and encryption options
BlobClient plaintextBlob;
ClientSideEncryptionOptions encryptionOptions;

// Get a copy of plaintextBlob that uses client-side encryption
BlobClient clientSideEncryptionBlob = plaintextBlob.WithClientSideEncryptionOptions(encryptionOptions);
```

# <a name="net-v11"></a>[Версии 11 .NET](#tab/dotnet11)
Создайте объект **BlobEncryptionPolicy** и задайте его в параметрах запроса (для каждого API или на уровне клиента с помощью параметра **DefaultRequestOptions** ). Все остальные задачи решаются клиентской библиотекой.

```csharp
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Upload the encrypted contents to the blob.
blob.UploadFromStream(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
MemoryStream outputStream = new MemoryStream();
blob.DownloadToStream(outputStream, null, options, null);
```

---

### <a name="queue-service-encryption"></a>Шифрование службы очередей
# <a name="net-v12"></a>[Платформа .NET версии 12](#tab/dotnet)
Создайте объект **клиентсидинкриптионоптионс** и настройте его при создании клиента с помощью **спеЦиализедкуеуеклиентоптионс**. Задать параметры шифрования для каждого API нельзя. Все остальные задачи решаются клиентской библиотекой.

```csharp
// Your key and key resolver instances, either through KeyVault SDK or an external implementation
IKeyEncryptionKey key;
IKeyEncryptionKeyResolver keyResolver;

// Create the encryption options to be used for upload and download.
ClientSideEncryptionOptions encryptionOptions = new ClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Set the encryption options on the client options
QueueClientOptions options = new SpecializedQueueClientOptions() { ClientSideEncryption = encryptionOptions };

// Get your queue client with client-side encryption enabled.
// Client-side encryption options are passed from service to queue clients.
QueueClient queue = new QueueServiceClient(connectionString, options).GetQueueClient("myQueue");

// Send an encrypted queue message.
queue.SendMessage("Hello, World!");

// Download queue messages, decrypting ones that are detected to be encrypted
QueueMessage[] queue.ReceiveMessages(); 
```

**Куеуесервицеклиент** не требуется применять параметры шифрования. Они также могут передаваться в конструкторы **QueueClient** , принимающие объекты **куеуеклиентоптионс** .

Если нужный объект **QueueClient** уже существует, но без параметров шифрования на стороне клиента, существует метод расширения для создания копии этого объекта с заданным **клиентсидинкриптионоптионс**. Этот метод расширения позволяет избежать издержек при создании нового объекта **QueueClient** с нуля.

```csharp
using Azure.Storage.Queues.Specialized;

// Your existing QueueClient instance and encryption options
QueueClient plaintextQueue;
ClientSideEncryptionOptions encryptionOptions;

// Get a copy of plaintextQueue that uses client-side encryption
QueueClient clientSideEncryptionQueue = plaintextQueue.WithClientSideEncryptionOptions(encryptionOptions);
```

Некоторые пользователи могут иметь очереди, где не все полученные сообщения могут быть успешно расшифрованы, а ключ или сопоставитель должны создаваться. В этом случае в последней строке приведенного выше примера будет создано исключение, и ни одно из полученных сообщений не будет доступно. В этих сценариях для предоставления параметров шифрования клиентам можно использовать подкласс **куеуеклиентсидинкриптионоптионс** . Он предоставляет **декриптионфаилед** событий, который активируется при каждом сбое расшифровки сообщения очереди, если в событие был добавлен хотя бы один вызов. Отдельные сообщения с ошибками могут обрабатываться таким образом, и они будут отфильтрованы из окончательного **куеуемессаже []** , возвращенного **рецеивемессажес**.

```csharp
// Create your encryption options using the sub-class.
QueueClientSideEncryptionOptions encryptionOptions = new QueueClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Add a handler to the DecryptionFailed event.
encryptionOptions.DecryptionFailed += (source, args) => {
   QueueMessage failedMessage = (QueueMessage)source;
   Exception exceptionThrown = args.Exception;
   // do something
};

// Use these options with your client objects.
QueueClient queue = new QueueClient(connectionString, queueName, new SpecializedQueueClientOptions()
{
   ClientSideEncryption = encryptionOptions
});

// Retrieve 5 messages from the queue.
// Assume 5 messages come back and one throws during decryption.
QueueMessage[] messages = queue.ReceiveMessages(maxMessages: 5).Value;
Debug.Assert(messages.Length == 4)
```

# <a name="net-v11"></a>[Версии 11 .NET](#tab/dotnet11)
Создайте объект **QueueEncryptionPolicy** и задайте его в параметрах запроса (для каждого API или на уровне клиента с помощью параметра **DefaultRequestOptions** ). Все остальные задачи решаются клиентской библиотекой.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

---

### <a name="table-service-encryption-v11-only"></a>Шифрование службы таблиц (только версии 11)
Помимо создания политики шифрования и настройки параметров запроса необходимо указать **EncryptionResolver** в параметрах **TableRequestOptions** или задать атрибут [EncryptProperty] для сущности.

#### <a name="using-the-resolver"></a>Использование сопоставителя

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need to specify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Использование атрибутов
Как было показано выше, если сущность реализует TableEntity, свойства можно декорировать с помощью атрибута [EncryptProperty], а не указав **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Шифрование и производительность
Обратите внимание, что шифрование результатов анализа данных хранилища отрицательно влияет на производительность. Ключ содержимого и вектор инициализации необходимо создать, само содержимое — зашифровать, а дополнительные метаданные — отформатировать и передать. Эти издержки зависят от объема шифруемых данных. Мы рекомендуем клиентам всегда тестировать свои приложения для повышения производительности во время разработки.

## <a name="next-steps"></a>Дальнейшие действия
* [Шифрование и расшифровка BLOB-объектов в хранилище Microsoft Azure с помощью хранилища ключей Azure](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
* Скачайте [Azure Storage Client Library for .NET NuGet package](https://www.nuget.org/packages/WindowsAzure.Storage)
* Скачайте пакеты NuGet [Core](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [Client](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/) и [Extensions](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) для хранилища ключей Azure.  
* Просмотрите [документацию по хранилищу ключей Azure](../../key-vault/general/overview.md)
