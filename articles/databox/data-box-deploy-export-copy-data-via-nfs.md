---
title: Учебник по копированию данных из Azure Data Box через NFS | Документация Майкрософт
description: Сведения о том, как копировать данные из Azure Data Box через NFS
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/01/2020
ms.author: alkohli
ms.openlocfilehash: bd8e6d4175c57bd31c3fd83bf6f9669d2b65ffb2
ms.sourcegitcommit: 487a9f5272300d60df2622c3d13e794d54680f90
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2020
ms.locfileid: "91660855"
---
# <a name="tutorial-copy-data-from-azure-data-box-via-nfs-preview"></a>Руководство по Копирование данных из Azure Data Box через NFS (предварительная версия)

В этом учебнике объясняется, как подключиться к локальному веб-интерфейсу Data Box и скопировать данные с такого устройства на локальный сервер данных через NFS. Данные на Data Box экспортируются из вашей учетной записи службы хранилища Azure.

В этом руководстве описано следующее:

> [!div class="checklist"]
>
> * Предварительные требования
> * подключение к Data Box;
> * Копирование данных из Data Box

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Предварительные требования

Перед тем как начать, убедитесь в следующем.

1. Вы разместили заказ на Azure Data Box.
    - Сведения о заказе на импорт см. в статье [Руководство по оформлению заказа на Azure Data Box](data-box-deploy-ordered.md).
    - Сведения о заказе на экспорт см. в статье [Руководство по оформлению заказа на Azure Data Box](data-box-deploy-export-ordered.md).
2. Вы получили Data Box. Состояние заказа на портале — **Доставлено**.
3. У вас есть главный компьютер, на который вы хотите скопировать данные из Data Box. На главном компьютере должно быть следующее ПО:
   * [поддерживаемая операционная система](data-box-system-requirements.md);
   * Компьютер должен быть подключен к высокоскоростной сети. Настоятельно рекомендуем использовать хотя бы одно соединение Ethernet со скоростью передачи данных 10 Гбит/с. Если подключение 10 Гбит/с недоступно, можно использовать канал передачи данных 1 Гбит/с, но при этом скорость копирования будет снижена.

## <a name="connect-to-data-box"></a>подключение к Data Box;

[!INCLUDE [data-box-shares](../../includes/data-box-shares.md)]

Если вы используете главный компьютер с ОС Linux, то выполните указанные ниже действия, чтобы настроить Data Box для доступа к NFS-клиентам. Data Box может одновременно подключаться к нескольким клиентам NFS (до пяти).

1. Укажите IP-адреса клиентов, которым разрешен доступ к общей папке.

    1.  В локальном пользовательском веб-интерфейсе перейдите на страницу **Подключение и копирование**. В разделе **Параметры NFS** щелкните **Клиентский доступ NFS**. 

        ![Открытие клиентского доступа NFS](media/data-box-deploy-export-copy-data/nfs-client-access-1.png)

    1. Чтобы добавить клиент NFS, укажите IP-адрес клиента и щелкните **Добавить**. Data Box может одновременно подключаться к нескольким клиентам NFS (до пяти). По завершении нажмите кнопку **ОК**.

         ![Добавление клиента NFS](media/data-box-deploy-export-copy-data/nfs-client-access-2.png)

2. Убедитесь, что на главном компьютере с ОС Linux установлен NFS-клиент [поддерживаемой версии](data-box-system-requirements.md). Используйте версию, подходящую для используемого вами дистрибутива ОС Linux. 

3. После установки NFS-клиента подключите общую папку NFS на устройстве Data Box, выполнив указанную ниже команду.

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    В примере ниже показано, как подключиться к общей папке Data Box по протоколу NFS. Устройство Data Box имеет IP-адрес `10.161.23.130`, общая папка `Mystoracct_Blob` подключена в виртуальной машине ubuntuVM, а точка подключения имеет название `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`
    
    Для клиентов Mac необходимо добавить дополнительный параметр следующим образом: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

    **Всегда создавайте отдельную папку для файлов, которые вы собираетесь скопировать в общую папку**. Папка, созданная в общих папках блочных и страничных BLOB-объектов, представляет собой контейнер, куда передаются данные в виде больших двоичных объектов. Вы не можете копировать файлы напрямую в *корневую* папку в учетной записи хранения.

## <a name="copy-data-from-data-box"></a>Копирование данных из Data Box

После подключения к общим папкам Data Box можно скопировать данные.

[!INCLUDE [data-box-export-review-logs](../../includes/data-box-export-review-logs.md)]

 Теперь вы можете начать копирование данных. Если у вас главный компьютер с ОС Linux, используйте программу копирования, аналогичную Robocopy. Вот некоторые программы, доступные в ОС Linux: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/), [Ultracopier](https://ultracopier.first-world.info/).  

Команда `cp` — один из лучших способов копировать каталоги. Дополнительные сведения об использовании команды см. на [страницах руководства команды cp](http://man7.org/linux/man-pages/man1/cp.1.html).

При копировании при помощи rsync с использованием нескольких потоков следуйте указанным ниже рекомендациям.

* Установите пакет **CIFS Utils** или **NFS Utils** (в зависимости от того, какая файловая система используется в клиенте с ОС Linux).

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

* Установите программу **Rsync** и **Parallel** (в зависимости от версии используемого дистрибутива ОС Linux).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

* Создайте точку подключения.

    `sudo mkdir /mnt/databox`

* Подключите том.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

* Сделайте зеркальное отражение структуры каталогов.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

* Скопируйте файлы.

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     здесь j — количество случаев распараллеливания, а X — количество параллельных копий

     Рекомендуем начать работу с 16 параллельных копий и увеличить количество потоков в зависимости от доступных ресурсов.

> [!IMPORTANT]
> Не поддерживаются следующие типы файлов Linux: символьные ссылки, символьные файлы, блочные файлы, сокеты и каналы. Эти типы файлов приведут к сбоям на этапе **Подготовка к отправке**.

По завершении копирования перейдите к **панели мониторинга** и проверьте использованное и свободное место на устройство.

Теперь вы можете отправить свое устройство Data Box в корпорацию Майкрософт.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве были освещены следующие темы относительно Azure Data Box.

> [!div class="checklist"]
>
> * Предварительные требования
> * подключение к Data Box;
> * копирование данных из Data Box.

Перейдите к следующему руководству, чтобы узнать, как отправить свой Data Box обратно в корпорацию Майкрософт.

> [!div class="nextstepaction"]
> [Отправка Azure Data Box в корпорацию Майкрософт](./data-box-deploy-export-picked-up.md)
