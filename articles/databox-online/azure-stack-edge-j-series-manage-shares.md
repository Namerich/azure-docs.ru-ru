---
title: Управление общими ресурсами GPU для Azure Stack ребра | Документация Майкрософт
description: Описывает, как использовать портал Azure для управления общими папками на графическом процессоре Azure Stack.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 413a93a145ae063a3aab4066ed62365e154d744a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2020
ms.locfileid: "96454104"
---
# <a name="use-azure-portal-to-manage-shares-on-your-azure-stack-edge-pro"></a>Использование портал Azure для управления общими папками на Azure Stack крае Pro

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

В этой статье описывается, как управлять общими папками на Azure Stack пограничных Pro. Вы можете управлять Azure Stack пограничным Pro через портал Azure или через локальный веб-интерфейс. С помощью портала Azure можно добавлять, удалять и обновлять общие папки или синхронизировать ключ хранилища для учетной записи хранения, связанной с общими папками.

## <a name="about-shares"></a>Сведения об общих папках

Чтобы передавать данные в Azure, необходимо создать общие папки на Azure Stack пограничных Pro. Общие папки, добавляемые на устройство Azure Stack ребра Pro, могут быть локальными общими папками или общими папками, которые принудительно отправляют данные в облако.

 - **Локальные общие папки**: Используйте эти общие папки, если требуется, чтобы данные обрабатывались локально на устройстве.
 - **Общие папки**: Используйте эти общие папки, если вы хотите, чтобы данные устройства автоматически отправлялись в учетную запись хранения в облаке. Все облачные функции, такие как **Обновление** и **ключи хранилища синхронизации** , применяются к общим ресурсам.


## <a name="add-a-share"></a>Добавление общей папки

Чтобы создать общую папку, выполните следующие действия на портале Azure.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите в раздел **> шлюзы Общие ресурсы**. На панели команд щелкните **+ Добавление общего файлового ресурса**.

    ![Выбор команды "Добавление общего файлового ресурса"](media/azure-stack-edge-j-series-manage-shares/add-share-1.png)

2. В разделе **Добавление общего файлового ресурса** укажите параметры общей папки. Укажите уникальное имя для общей папки.
    
    Имена общих папок могут содержать только цифры, строчные буквы и дефисы. Имя общей папки может содержать от 3 до 63 знаков и начинаться с буквы или цифры. Перед каждым дефисом должен быть знак без дефиса.

3. Выберите **тип** для общей папки. Вы можете выбрать тип **SMB** или **NFS** (тип SMB задан по умолчанию). SMB является стандартным для клиентов Windows, а NFS используется для клиентов Linux. В зависимости от того, выбраны ли общие папки типа SMB или NFS, предложенные варианты могут немного отличаться.

4. Предоставьте **учетную запись хранения**, в которой будет храниться общая папка. При отсутствии контейнера он создается в учетной записи хранения с именем общей папки. Если контейнер уже существует, используется имеющийся контейнер.

5. В раскрывающемся списке выберите **службу хранилища** из блочного большого двоичного объекта, страничного BLOB-объекта или файлов. Тип выбранной службы зависит от того, в каком формате необходимо хранить данные в Azure. Например, в этом экземпляре мы хотим, чтобы данные находились как блочные BLOB-объекты в Azure, поэтому мы выбираем **блочный BLOB-объект**. При выборе **страничного BLOB-объекта** необходимо обеспечить согласованность данных в 512 байт. Используйте **страничный BLOB-объект** для виртуальных жестких дисков или VHDX, размер которых всегда составляет 512 байт.

6. Этот шаг зависит от того, создается ли общая папка типа SMB или NFS.
    - **При создании общей папки SMB** в поле **Все привилегированные локальные пользователи** выберите **Создать** или **Использовать существующий**. При создании локального пользователя укажите **имя пользователя** и **пароль**, а затем подтвердите пароль. Таким образом вы предоставите разрешения локальному пользователю. Назначив разрешения здесь, вы сможете изменять их в проводнике.

        ![Добавление общей папки SMB](media/azure-stack-edge-j-series-manage-shares/add-smb-share.png)

        Если разрешить только операции чтения для данных этой общей папки, можно указать пользователей с правами только для чтения.
    - **При создании общей папки типа NFS** необходимо предоставить **IP-адреса разрешенных клиентов**, которые могут получать доступ к ресурсу.

        ![Добавление общей папки типа NFS](media/azure-stack-edge-j-series-manage-shares/add-nfs-share.png)

7. Для получения легкого доступа к общим папкам из модулей вычислений Edge используйте локальную точку подключения. Выберите **Использовать общую папку с пограничными вычислениями**, чтобы общая папка автоматически подключалась после создания. Этот параметр позволяет модулю Edge использовать вычисления с локальной точкой подключения.

8. Нажмите кнопку **создать** , чтобы создать общую папку. Вы получите уведомление о том, что общая папка создается. После создания общей папки с указанными параметрами колонка **общих ресурсов** обновляется в соответствии с новой общей папкой.

## <a name="add-a-local-share"></a>Добавление локальной общей папки

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите в раздел **> шлюзы Общие ресурсы**. На панели команд щелкните **+ Добавление общего файлового ресурса**.

    ![Выберите Добавить общий ресурс 2](media/azure-stack-edge-j-series-manage-shares/add-local-share-1.png)

2. В разделе **Добавление общего файлового ресурса** укажите параметры общей папки. Укажите уникальное имя для общей папки.
    
    Имена общих ресурсов могут содержать только цифры, строчные и прописные буквы, а также дефисы. Имя общей папки может содержать от 3 до 63 знаков и начинаться с буквы или цифры. Перед каждым дефисом должен быть знак без дефиса.

3. Выберите **тип** для общей папки. Вы можете выбрать тип **SMB** или **NFS** (тип SMB задан по умолчанию). SMB является стандартным для клиентов Windows, а NFS используется для клиентов Linux. В зависимости от того, выбраны ли общие папки типа SMB или NFS, предложенные варианты могут немного отличаться.

   > [!IMPORTANT]
   > Убедитесь, что требуемая учетная запись хранения Azure не имеет определенных политик неизменяемости, если она используется с устройством Azure Stack Edge Pro или Шлюза Data Box. См. сведения об [определении и администрировании политик неизменяемости для хранилища BLOB-объектов](../storage/blobs/storage-blob-immutability-policies-manage.md).

4. Для получения легкого доступа к общим папкам из модулей вычислений Edge используйте локальную точку подключения. Чтобы модуль Edge мог использовать вычисление с локальной точкой подключения, выберите **Use the share with Edge compute** (Использование общей папки с вычислениями Edge).

5. Выберите **Configure as Edge local shares** (Настройка в качестве локальных общих папок Edge). Данные в локальных общих папках останутся локально на устройстве. Вы можете обрабатывать эти данные локально.

6. В поле **Все привилегированные локальные пользователи** выберите либо **Создать**, либо **Использовать существующую**.

7. Нажмите кнопку **создания**. 

    ![Создание локальной общей папки](media/azure-stack-edge-j-series-manage-shares/add-local-share-2.png)

    Вы получите уведомление о том, что создается общая папка. После создания общей папки с указанными параметрами колонка **общих ресурсов** обновляется в соответствии с новой общей папкой.

    ![Просмотр обновлений колонки "Общие папки"](media/azure-stack-edge-j-series-manage-shares/add-local-share-3.png)
    
    Выберите локальную общую папку, чтобы просмотреть локальную точку подключения для модулей вычислений Edge для этого общего ресурса.

    ![Просмотр общих сведений локальной общей папки](media/azure-stack-edge-j-series-manage-shares/add-local-share-4.png)

## <a name="mount-a-share"></a>Подключение общей папки

Если вы создали общий ресурс до того, как вы настроили вычисление на устройстве Azure Stack пограничном Pro, вам потребуется подключить общую папку. Для этого выполните следующие шаги.


1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите в раздел **> шлюзы Общие ресурсы**. В списке общих папок выберите ту, которую необходимо подключить. Для выбранной общей папки в столбце **Используется для вычислений** отобразится состояние **Отключено**.

    ![Выбор общей папки](media/azure-stack-edge-j-series-manage-shares/mount-share-1.png)

2. Выберите **Подключить**.

    ![Выбор параметра "Подключить"](media/azure-stack-edge-j-series-manage-shares/mount-share-2.png)

3. При появлении запроса на подтверждение нажмите кнопку **Да**. В этом случае общая папка будет подключена.

    ![Подтверждение подключения](media/azure-stack-edge-j-series-manage-shares/mount-share-3.png)

4. После подключения перейдите к списку общих папок. Вы увидите, что в столбце **Используется для вычислений** в качестве состояния общей папки отображается **Включено**.

    ![Общая папка подключена](media/azure-stack-edge-j-series-manage-shares/mount-share-4.png)

5. Снова выберите общую папку, чтобы просмотреть ее локальную точку подключения. Модуль пограничных вычислений использует эту локальную точку подключения для общей папки.

    ![Локальная точка подключения для общей папки](media/azure-stack-edge-j-series-manage-shares/mount-share-5.png)

## <a name="unmount-a-share"></a>Отключение общей папки

Чтобы отключить общую папку, выполните следующие действия на портале Azure.

1. В портал Azure перейдите к ресурсу Azure Stackного периметра и перейдите в раздел **> шлюзы Общие ресурсы**.

    ![Выберите общий ресурс 2](media/azure-stack-edge-j-series-manage-shares/unmount-share-1.png)

2. В списке общих папок выберите папку, которую требуется отключить. Убедитесь, что она не используется модулем. Если же общая папка используется модулем, это приведет к возникновению проблем с модулем. Выберите **Отключить**.

    ![Выбор параметра "Отключить"](media/azure-stack-edge-j-series-manage-shares/unmount-share-2.png)

3. При появлении запроса на подтверждение нажмите кнопку **Да**. В этом случае общая папка будет отключена.

    ![Подтверждение отключения](media/azure-stack-edge-j-series-manage-shares/unmount-share-3.png)

4. После отключения перейдите к списку общих папок. Вы увидите, что в столбце **Используется для вычислений** в качестве состояния общей папки отображается **Отключено**.

    ![Общая папка отключена](media/azure-stack-edge-j-series-manage-shares/unmount-share-4.png)

## <a name="delete-a-share"></a>удаление общей папки.

Чтобы удалить общую папку, выполните следующие действия на портале Azure.

1. В списке общих папок выберите и щелкните папку, которую требуется удалить.

    ![Выберите общий ресурс 3](media/azure-stack-edge-j-series-manage-shares/delete-share-1.png)

2. Щелкните **Удалить**.

    ![Нажмите кнопку "Удалить"](media/azure-stack-edge-j-series-manage-shares/delete-share-2.png)

3. При запросе подтверждения нажмите кнопку **Да**.

    ![Подтверждение удаления](media/azure-stack-edge-j-series-manage-shares/delete-share-3.png)

Список общих папок обновляется с учетом удаления.

## <a name="refresh-shares"></a>Обновление общих папок

Возможность обновления позволяет обновить содержимое общей папки. При обновлении общей папки будет инициирован поиск всех объектов Azure, включая большие двоичные объекты и файлы, добавленные в облако с момента последнего обновления. Затем эти дополнительные файлы скачиваются для обновления содержимого общей папки на устройстве.

> [!IMPORTANT]
> - Вы не можете обновить локальные общие папки.
> - Разрешения и списки управления доступом (ACL) не сохраняются при операции обновления. 

Чтобы обновить общую папку, выполните следующие действия на портале Azure.

1.  На портале Azure перейдите к разделу **Общие папки**. Выберите и щелкните общую папку, которую требуется обновить.

    ![Выберите общий доступ 4](media/azure-stack-edge-j-series-manage-shares/refresh-share-1.png)

2.  Нажмите кнопку **Обновить**. 

    ![Нажатие кнопки обновления](media/azure-stack-edge-j-series-manage-shares/refresh-share-2.png)
 
3.  При запросе подтверждения нажмите кнопку **Да**. Запустится задание обновления содержимого локальной общей папки.

    ![Подтверждение обновления](media/azure-stack-edge-j-series-manage-shares/refresh-share-3.png)
 
4.   Во время обновления в контекстном меню недоступна функция обновления. Щелкните уведомление задания, чтобы просмотреть состояние задания обновления.

5.   Время обновления зависит от количества файлов в контейнере Azure и на устройстве. После успешного обновления общей папки обновляется ее метка времени. Даже если во время обновления произошли частичные сбои, операция считается успешной и метка времени обновляется. Последние версии журналов ошибок также обновляются.

![Обновленная метка времени](media/azure-stack-edge-j-series-manage-shares/refresh-share-4.png)
 
Если возникает сбой, создается предупреждение. В этом предупреждении подробно описываются причина его возникновения и рекомендации для устранения проблем. Предупреждение также содержит ссылки на файл, содержащий всю сводку сбоев, включая файлы, которые не удалось обновить или удалить.

## <a name="sync-pinned-files"></a>Синхронизировать закрепленные файлы 

Чтобы автоматически синхронизировать закрепленные файлы, выполните следующие действия в портал Azure.
 
1. Выберите существующую учетную запись хранения Azure. 

2. Перейдите в **контейнеры** и выберите **+ контейнер** , чтобы создать контейнер. Присвойте этому контейнеру имя *невконтаинер*. Установите для **общего уровня доступа** значение контейнер.

    ![Автоматическая синхронизация закрепленных файлов 1](media/azure-stack-edge-j-series-manage-shares/image-1.png)

3. Выберите имя контейнера и задайте следующие метаданные:  

    - Name = "закрепленный" 
    - Значение = "true" 

    ![Автоматическая синхронизация для закрепленных файлов 2](media/azure-stack-edge-j-series-manage-shares/image-2.png)
 
4. Создайте на устройстве новую общую папку. Сопоставьте его с закрепленным контейнером, выбрав параметр существующий контейнер. Пометка общего ресурса как доступного только для чтения. Создайте нового пользователя и укажите имя пользователя и соответствующий пароль для этой общей папки.  

    ![Автоматическая синхронизация закрепленных файлов 3](media/azure-stack-edge-j-series-manage-shares/image-3.png)
 
5. В портал Azure перейдите к созданному контейнеру. Отправьте файл, который необходимо закрепить в невконтаинер, для которого зафиксированы метаданные. 

6. Выберите **Обновить данные** в портал Azure, чтобы устройство скачало политику закрепления для этого контейнера хранилища Azure.  

    ![Автоматическая синхронизация закрепленных файлов 4](media/azure-stack-edge-j-series-manage-shares/image-4.png)
 
7. Получите доступ к новому общему ресурсу, созданному на устройстве. Файл, который был отправлен в учетную запись хранения, теперь загружается в локальный общий ресурс. 

    Когда устройство отключается или повторно подключается, оно активирует обновление. При обновлении будут переноситься только те файлы, которые были изменены. 


## <a name="sync-storage-keys"></a>Синхронизация ключей хранилища

Если ключи учетной записи хранения изменены, необходимо синхронизировать ключи доступа к хранилищу. Путем выполнения синхронизации можно получить последние ключи для учетной записи устройства.

Выполните следующие действия на портале Azure, чтобы синхронизировать ключ доступа к хранилищу.

1. В своем ресурсе перейдите к разделу **Обзор**. Из списка общих папок выберите и щелкните ту, которая связана с учетной записью хранения и которую необходимо синхронизировать.

    ![Выбор общей папки с соответствующей учетной записью хранения](media/azure-stack-edge-j-series-manage-shares/sync-storage-key-1.png)

2. Щелкните **Sync storage key** (Синхронизация ключа хранилища). Нажмите кнопку **ОК** в запросе на подтверждение.

     ![Выбор синхронизации ключа хранилища](media/azure-stack-edge-j-series-manage-shares/sync-storage-key-2.png)

3. После синхронизации закройте диалоговое окно.

>[!NOTE]
> Это действие достаточно выполнить один раз для каждой учетной записи хранения. Не нужно повторять это действие для всех общих папок, связанных с одной учетной записью.


## <a name="next-steps"></a>Дальнейшие действия

- Узнайте, как [управлять пользователями с помощью портала Azure](azure-stack-edge-j-series-manage-users.md).