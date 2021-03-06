---
title: Использование хранилища очередей Azure из Python в службе хранилища Azure
description: Узнайте, как использовать служба очередей Azure из Python для создания и удаления очередей, а также для вставки, получения и удаления сообщений.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/25/2020
ms.service: storage
ms.subservice: queues
ms.topic: how-to
ms.reviewer: dineshm
ms.custom: seo-javascript-october2019, devx-track-python
ms.openlocfilehash: ac75b89548d346945901d752672ef0f08601ccfb
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93345658"
---
# <a name="how-to-use-azure-queue-storage-from-python"></a>Использование хранилища очередей Azure из Python

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>Обзор

В этой статье описываются распространенные сценарии использования службы хранилища очередей Azure. В число рассматриваемых сценариев входят Вставка, просмотр, получение и удаление сообщений очереди. Также рассматривается код для создания и удаления очередей.

Примеры в этой статье написаны на языке Python и используют [клиентскую библиотеку хранилища очередей Azure для Python]. Дополнительные сведения об очередях см. в разделе [Дальнейшие действия](#next-steps).

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Загрузите и установите пакет SDK службы хранилища Azure для Python.

[Для пакета SDK службы хранилища Azure для Python](https://github.com/azure/azure-storage-python) требуется Python версии 2,7, 3,3 или более поздней.

### <a name="install-via-pypi"></a>Установка через PyPI

Для установки с помощью индекса пакетов Python (PyPI) введите:

# <a name="python-v12"></a>[Python версии 12](#tab/python)

```console
pip install azure-storage-queue
```

# <a name="python-v2"></a>[Python v2](#tab/python2)

```console
pip install azure-storage-queue==2.1.0
```

---

> [!NOTE]
> Если вы обновляете пакет SDK службы хранилища Azure для Python версии 0.36 или более ранней, удалите старый пакет SDK с помощью `pip uninstall azure-storage`, прежде чем устанавливать последнюю версию пакета.

Альтернативные способы установки см. в статье [пакет Azure SDK для Python].

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="configure-your-application-to-access-queue-storage"></a>Настройка приложения для доступа к хранилищу очередей

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Объект [QueueClient](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient) позволяет работать с очередью. Добавьте следующий код в начало любого файла Python, в котором вы хотите получить программный доступ к очереди Azure:

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_ImportStatements":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Объект [QueueService](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2) позволяет работать с очередями. Следующий код создает `QueueService` объект. Добавьте следующий код в начало любого файла Python, в котором вы хотите получить доступ к службе хранилища Azure программным способом.

```python
from azure.storage.queue import (
        QueueService,
        QueueMessageFormat
)

import os, uuid
```

---

`os`Пакет обеспечивает поддержку для получения переменной среды. `uuid`Пакет обеспечивает поддержку создания уникального идентификатора для имени очереди.

## <a name="create-a-queue"></a>Создание очереди

Строка подключения извлекается из `AZURE_STORAGE_CONNECTION_STRING` переменной среды, установленной ранее.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Следующий код создает объект, `QueueClient` используя строку подключения к хранилищу.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_CreateQueue":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Следующий код создает объект, `QueueService` используя строку подключения к хранилищу.

```python
# Retrieve the connection string from an environment
# variable named AZURE_STORAGE_CONNECTION_STRING
connect_str = os.getenv("AZURE_STORAGE_CONNECTION_STRING")

# Create a unique name for the queue
queue_name = "queue-" + str(uuid.uuid4())

# Create a QueueService object which will
# be used to create and manipulate the queue
print("Creating queue: " + queue_name)
queue_service = QueueService(connection_string=connect_str)

# Create the queue
queue_service.create_queue(queue_name)
```

---

## <a name="insert-a-message-into-a-queue"></a>Вставка сообщения в очередь

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Чтобы вставить сообщение в очередь, используйте метод [send_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-) .

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_AddMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Чтобы вставить сообщение в очередь, используйте метод [put_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#put-message-queue-name--content--visibility-timeout-none--time-to-live-none--timeout-none-) , чтобы создать новое сообщение и добавить его в очередь.

```python
message = u"Hello World"
print("Adding message: " + message)
queue_service.put_message(queue_name, message)
```

---

Сообщения в очереди Azure хранятся в виде текста. Если вы хотите хранить двоичные данные, настройте функции кодирования и декодирования Base64 перед помещением сообщения в очередь.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Настройка функций кодирования и декодирования Base64 в клиентском объекте очереди.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_EncodeMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Настройка функций кодирования и декодирования Base64 для объекта службы очередей.

```python
# Setup Base64 encoding and decoding functions
queue_service.encode_function = QueueMessageFormat.binary_base64encode
queue_service.decode_function = QueueMessageFormat.binary_base64decode
```

---

## <a name="peek-at-messages"></a>Просмотр сообщений

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Вы можете просматривать сообщения, не удаляя их из очереди, вызывая метод [peek_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#peek-messages-max-messages-none----kwargs-) . По умолчанию `peek_messages` просматривает одно сообщение.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_PeekMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Вы можете просматривать сообщения, не удаляя их из очереди, вызывая метод [peek_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#peek-messages-queue-name--num-messages-none--timeout-none-) . По умолчанию `peek_messages` просматривает одно сообщение.

```python
messages = queue_service.peek_messages(queue_name)

for peeked_message in messages:
    print("Peeked message: " + peeked_message.content)
```

---

## <a name="change-the-contents-of-a-queued-message"></a>Изменение содержимого сообщения в очереди

Вы можете изменить содержимое сообщения непосредственно в очереди. Если сообщение представляет задачу, эту функцию можно использовать для обновления состояния задачи.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

В приведенном ниже коде используется метод [update_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#update-message-message--pop-receipt-none--content-none----kwargs-) для обновления сообщения. Время ожидания видимости равно 0. Это означает, что сообщение появится немедленно, и содержимое обновится.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_ChangeMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

В приведенном ниже коде используется метод [update_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#update-message-queue-name--message-id--pop-receipt--visibility-timeout--content-none--timeout-none-) для обновления сообщения. Время ожидания видимости равно 0. Это означает, что сообщение появится немедленно, и содержимое обновится.

```python
messages = queue_service.get_messages(queue_name)

for message in messages:
    queue_service.update_message(
        queue_name, message.id, message.pop_receipt, 0, u"Hello World Again")
```

---

## <a name="get-the-queue-length"></a>Получение длины очереди

Вы можете узнать приблизительное количество сообщений в очереди.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Метод [get_queue_properties](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#get-queue-properties---kwargs-) запрашивает у службы очередей возвращаемые свойства очереди, включая `approximate_message_count` .

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_GetQueueLength":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Метод [get_queue_metadata](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#get-queue-metadata-queue-name--timeout-none-) запрашивает у службы очередей получение метаданных об очереди, включая `approximate_message_count` .

```python
metadata = queue_service.get_queue_metadata(queue_name)
count = metadata.approximate_message_count
print("Message count: " + str(count))
```

---

В результате число указывается лишь приблизительно, так как сообщения могут добавляться или удаляться после ответа службы очередей на ваш запрос.

## <a name="dequeue-messages"></a>Вывод сообщений из очереди

Удаление сообщения из очереди в два этапа. Если код не может обработать сообщение, этот двухэтапный процесс гарантирует, что вы получите то же сообщение и повторите попытку. Вызов `delete_message` после успешной обработки сообщения.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

При вызове [receive_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-)вы получаете следующее сообщение в очереди по умолчанию. Сообщение, возвращаемое методом `receive_messages`, становится невидимым для другого кода, считывающего сообщения из этой очереди. По умолчанию это сообщение остается невидимым в течение 30 секунд. Чтобы завершить удаление сообщения из очереди, необходимо также вызвать [delete_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#delete-message-message--pop-receipt-none----kwargs-).

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DequeueMessages":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

При вызове [get_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-)вы получаете следующее сообщение в очереди по умолчанию. Сообщение, возвращаемое методом `get_messages`, становится невидимым для другого кода, считывающего сообщения из этой очереди. По умолчанию это сообщение остается невидимым в течение 30 секунд. Чтобы завершить удаление сообщения из очереди, необходимо также вызвать [delete_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#delete-message-queue-name--message-id--pop-receipt--timeout-none-).

```python
messages = queue_service.get_messages(queue_name)

for message in messages:
    print("Deleting message: " + message.content)
    queue_service.delete_message(queue_name, message.id, message.pop_receipt)
```

---

Способ извлечения сообщения из очереди можно настроить двумя способами. Во-первых, можно получить пакет сообщений (до 32 сообщений). Во-вторых, можно задать более длительное или короткое время ожидания видимости, чтобы предоставить коду больше или меньше времени на полную обработку каждого сообщения.

# <a name="python-v12"></a>[Python версии 12](#tab/python)

В следующем примере кода используется метод [receive_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) для получения сообщений в пакетах. Затем он обрабатывает каждое сообщение в каждом пакете с помощью вложенного `for` цикла. Он также задает время ожидания невидимости 5 минут для каждого сообщения.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DequeueByPage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

В следующем примере кода используется метод [get_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-) для получения 16 сообщений в одном вызове. Затем он обрабатывает каждое сообщение с помощью цикла `for`. Он также задает время ожидания невидимости 5 минут для каждого сообщения.

```python
messages = queue_service.get_messages(queue_name, num_messages=16, visibility_timeout=5*60)

for message in messages:
    print("Deleting message: " + message.content)
    queue_service.delete_message(queue_name, message.id, message.pop_receipt)
```

---

## <a name="delete-a-queue"></a>Удаление очереди

# <a name="python-v12"></a>[Python версии 12](#tab/python)

Чтобы удалить очередь и все сообщения, содержащиеся в ней, вызовите метод [delete_queue](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#delete-queue---kwargs-) .

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DeleteQueue":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Чтобы удалить очередь и все сообщения, содержащиеся в ней, вызовите метод [delete_queue](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2#delete-queue-queue-name--fail-not-exist-false--timeout-none-) .

```python
print("Deleting queue: " + queue_name)
queue_service.delete_queue(queue_name)
```

---

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали основные сведения о хранилище очередей, воспользуйтесь следующими ссылками для получения дополнительных сведений.

- [Справочник по API Python для очередей Azure](/python/api/azure-storage-queue)
- [Центр по разработке для Python](https://azure.microsoft.com/develop/python/)
- [API-интерфейс REST служб хранилища Azure](/rest/api/storageservices/)

[Клиентская библиотека хранилища очередей Azure для Python]: https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue
[SDK Azure для Python]: https://github.com/azure/azure-sdk-for-python
[Azure Storage Team Blog]: https://techcommunity.microsoft.com/t5/azure-storage/bg-p/AzureStorageBlog
