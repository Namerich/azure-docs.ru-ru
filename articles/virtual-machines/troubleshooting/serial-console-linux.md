---
title: Последовательная консоль Azure для Linux | Документация Майкрософт
description: Bi-Directional последовательной консоли для виртуальных машин Azure и масштабируемых наборов виртуальных машин с помощью примера Linux.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 5/1/2019
ms.author: alsin
ms.openlocfilehash: 25e3a9cb363ae4e64b953aeb7a6da4e2e66c9fc7
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977105"
---
# <a name="azure-serial-console-for-linux"></a>Последовательная консоль Azure для Linux

Последовательная консоль в портал Azure предоставляет доступ к текстовой консоли для виртуальных машин Linux и экземпляров масштабируемых наборов виртуальных машин. Это последовательное подключение подключается к последовательному порту ttyS0 ВИРТУАЛЬНОЙ машины или экземпляра масштабируемого набора виртуальных машин, предоставляя доступ к нему независимо от состояния сети или операционной системы. Доступ к Серийной консоли можно получить только с помощью портала Azure. Он разрешен только для тех пользователей, которые имеют роль доступа "Участник" или более высокого уровня для виртуальной машины или масштабируемого набора виртуальных машин.

Серийная консоль работает аналогичным образом для виртуальных машин и экземпляров масштабируемых наборов виртуальных машин. В этом документе все упоминания виртуальных машин будут неявно включать экземпляры масштабируемых наборов виртуальных машин, если не указано иное.

Последовательная консоль общедоступна в глобальных регионах Azure и общедоступной предварительной версии в Azure для государственных организаций. Она пока недоступна в облаке Azure для Китая.

Документацию по последовательной консоли для Windows см. в разделе [последовательная консоль для Windows](./serial-console-windows.md).

> [!NOTE]
> Последовательная консоль в настоящее время несовместима с управляемой учетной записью хранения для диагностики загрузки. Чтобы использовать последовательное консоль, убедитесь, что используется Настраиваемая учетная запись хранения.

## <a name="prerequisites"></a>Предварительные требования

- Виртуальная машина или экземпляр масштабируемого набора виртуальных машин должны использовать модель развертывания управления ресурсами. Классические развертывания не поддерживаются.

- Учетной записи, использующей последовательную консоль, необходимо присвоить [роль участника виртуальной машины](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) для виртуальной машины и учетной записи хранения [диагностики загрузки](boot-diagnostics.md).

- У виртуальной машины или экземпляра масштабируемого набора виртуальных машин должен быть пользователь с паролем. Вы можете создать ее с функцией [сброса пароля](../extensions/vmaccess.md#reset-password) расширения для доступа к виртуальной машине. В разделе **Поддержка и устранение неполадок** выберите **Сбросить пароль**.

- Для виртуальной машины или экземпляра масштабируемого набора виртуальных машин должна быть включена [Диагностика загрузки](boot-diagnostics.md) .

    ![Параметры диагностики загрузки](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Сведения о параметрах, относящихся к дистрибутивам Linux, см. в разделе [Доступность дистрибутива Linux для последовательной консоли](#serial-console-linux-distribution-availability).

- Для виртуальной машины или экземпляра масштабируемого набора виртуальных машин необходимо настроить для последовательного вывода `ttys0` . Это значение по умолчанию для образов Azure, но вы хотите установить его еще раз на пользовательских изображениях. Подробные сведения см. [ниже](#custom-linux-images).


> [!NOTE]
> Для последовательной консоли требуется локальный пользователь с настроенным паролем. Виртуальные машины или масштабируемые наборы виртуальных машин, настроенные только с помощью открытого ключа SSH, не смогут войти в последовательную консоль. Чтобы создать локального пользователя с паролем, используйте [расширение доступа к виртуальной машине](../extensions/vmaccess.md), доступное на портале Azure при выборе **Сброс пароля**.
> Кроме того, вы можете сбросить пароль администратора в своей учетной записи, [используя GRUB, чтобы загрузить однопользовательский режим](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Доступность дистрибутива последовательной консоли Linux
Для правильной работы последовательной консоли гостевая операционная система должна быть настроена для чтения и записи сообщений консоли в последовательный порт. В большинстве поддерживаемых [дистрибутивов Azure Linux](../linux/endorsed-distros.md) по умолчанию настроена последовательная консоль. Чтобы получить доступ к последовательной консоли, в разделе **Поддержка и устранение неполадок** выберите **Последовательная консоль**.

> [!NOTE]
> Если в последовательной консоли ничего не отображается, убедитесь, что на виртуальной машине включена диагностика загрузки. Нажатие клавиши **Ввод** часто приводит к устранению проблем, когда в последовательной консоли ничего не отображается.

Distribution      | Доступ к последовательной консоли
:-----------|:---------------------
Red Hat Enterprise Linux    | Доступ к последовательной консоли включен по умолчанию.
CentOS      | Доступ к последовательной консоли включен по умолчанию.
Debian      | Доступ к последовательной консоли включен по умолчанию.
Ubuntu      | Доступ к последовательной консоли включен по умолчанию.
CoreOS      | Доступ к последовательной консоли включен по умолчанию.
SUSE        | В новых образах SLES, доступных в Azure, доступ к последовательной консоли включен по умолчанию. Чтобы включить последовательную консоль при использовании более старых версий (10 или более ранних) SLES в Azure, ознакомьтесь со [статьей базы знаний](https://www.novell.com/support/kb/doc.php?id=3456486).
Oracle Linux        | Доступ к последовательной консоли включен по умолчанию.

### <a name="custom-linux-images"></a>Пользовательские образы Linux
Чтобы включить последовательную консоль для настраиваемого образа виртуальной машины Linux, включите доступ к консоли в файле */etc/inittab* для запуска терминала на `ttyS0`. Например: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Также может потребоваться порождение жетти на ttyS0. Это можно сделать с помощью `systemctl start serial-getty@ttyS0.service` .

Кроме того, необходимо добавить ttyS0 в качестве назначения для последовательного вывода. Дополнительные сведения о настройке пользовательского образа для работы с последовательной консолью см. в общих системных требованиях на странице [Создание и передача виртуального жесткого диска Linux в Azure](../linux/create-upload-generic.md#general-linux-system-requirements).

Если вы создаете настраиваемое ядро, советуем включить флаги `CONFIG_SERIAL_8250=y` и `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Файл конфигурации обычно находится в пути */boot/*.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Распространенные сценарии для доступа к последовательной консоли

Сценарий          | Действия в последовательной консоли
:------------------|:-----------------------------------------
Неработающий файл *FSTAB* | Нажмите клавишу **ВВОД**, чтобы продолжить, и исправьте *FSTAB*-файл с помощью текстового редактора. Для этого вам может потребоваться перейти в однопользовательский режим. Дополнительные сведения см. в разделе последовательной консоли [Устранение неполадок fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) и [Использование последовательной консоли для доступа к GRUB и однопользовательском режиме](serial-console-grub-single-user-mode.md).
Неверные правила брандмауэра |  Если вы настроили iptables для блокировки подключения SSH, вы можете использовать последовательную консоль для взаимодействия с виртуальной машиной без необходимости использования SSH. Дополнительные сведения можно найти на [справочной странице iptables](https://linux.die.net/man/8/iptables).<br>Аналогично, если брандмауэр блокирует доступ по протоколу SSH, вы можете получить доступ к виртуальной машине через последовательную консоль и перенастроить брандмауэр. Дополнительные сведения можно найти в [документации по брандмауэру](https://firewalld.org/documentation/).
Проверка файловой системы на случай повреждения | Дополнительные сведения об устранении неполадок в поврежденных файловых системах с помощью последовательной консоли см. в разделе последовательной консоли [виртуальной машины Linux в Azure](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) .
Проблемы с конфигурацией SSH | Откройте последовательную консоль и измените параметры. Серийная консоль можно использовать независимо от конфигурации SSH виртуальной машины, так как она не требует, чтобы виртуальная машина работала с сетевым подключением. Руководство по устранению неполадок можно найти в статье [Устранение неполадок SSH-подключений к виртуальной машине Azure под управлением Linux, которая завершилась ошибкой, выдает ошибку или отклоняется](./troubleshoot-ssh-connection.md). Дополнительные сведения см. в [подробных инструкциях по устранению неполадок SSH для проблем подключения к виртуальной машине Linux в Azure](./detailed-troubleshoot-ssh-connection.md) .
Взаимодействие с загрузчиком | Перезапустите виртуальную машину из колонки последовательной консоли для доступа к GRUB на виртуальной машине Linux. Дополнительные сведения и сведения о дистрибутив см. в статье [Использование последовательной консоли для доступа к GRUB и однопользовательским режимам](serial-console-grub-single-user-mode.md).

## <a name="disable-the-serial-console"></a>Отключение Серийной консоли

По умолчанию доступ к Серийной консоли включен во всех подписках. Серийную консоль можно отключить на уровне подписки или на уровне виртуальной машины или масштабируемого набора виртуальных машин. Подробные инструкции см. в статье [о включении и отключении Серийной консоли Azure](./serial-console-enable-disable.md).

## <a name="serial-console-security"></a>Безопасность последовательной консоли

### <a name="access-security"></a>Безопасность доступа
Доступ к последовательной консоли ограничен пользователями, которые имеют [роли с доступом участника виртуальной машины](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) или выше. Если клиент Azure Active Directory требует многофакторной проверки подлинности (MFA), то для доступа к последовательной консоли также понадобится MFA, так как доступ к последовательной консоли предоставляется через [портал Azure](https://portal.azure.com).

### <a name="channel-security"></a>Безопасность канала
Все данные, исходящие и поступающие, при пересылке по сети зашифрованы.

### <a name="audit-logs"></a>Журналы аудита
Все сведения о доступе к последовательной консоли записываются в журналы [диагностики загрузки](./boot-diagnostics.md) на виртуальной машине. Права доступа к этим журналам и на работу с ними принадлежат администратору виртуальной машины Azure.

> [!CAUTION]
> Пароли доступа для консоли не записываются. Тем не менее если команды, выполняемые в консоли, содержат или выводят пароли, секреты, имена пользователей или другие персональные данные в любой форме, они будут записываться в журналы диагностики загрузки виртуальной машины. Персональные данные будут записываться вместе с другим видимым текстом как часть реализации функции обратной прокрутки последовательной консоли. Эти журналы являются циклическими. Только пользователи с разрешениями на чтение в учетной записи хранения диагностики имеют к ним доступ. При размещении каких-либо команд, содержащих секреты или личные данные, рекомендуется использовать SSH, если последовательное использование консоли не является обязательным.

### <a name="concurrent-usage"></a>Одновременное использование
Когда один пользователь подключен к последовательной консоли, а другой успешно запрашивает доступ к этой же виртуальной машине, первый пользователь будет отключен, а второй — подключен к тому же сеансу.

> [!CAUTION]
> Это означает, что пользователь, который отключился, не будет входить в систему. Возможность принудительного выхода из системы при отключении (с помощью СИГХУП или аналогичного механизма) по-прежнему находится в стратегии. Для Windows включено автоматическое истечение времени ожидания в специальной административной консоли, а для Linux можно настроить параметр времени ожидания терминала. Чтобы сделать это, добавьте `export TMOUT=600` в файл *.bash_profile* или *.profile* для пользователя, которого вы используете для входа в консоль. При использовании этого параметра время действия сеанса истечет через 10 минут.

## <a name="accessibility"></a>Специальные возможности
Специальные возможности — ключевой фокус для последовательной консоли Azure. Для этого мы убедились, что последовательная консоль полностью доступна.

### <a name="keyboard-navigation"></a>Навигация с помощью клавиатуры
Используйте клавишу **TAB** на клавиатуре для навигации по интерфейсу последовательной консоли на портале Azure. Ваше местоположение будет выделено на экране. Чтобы оставить фокус в окне последовательной консоли, нажмите клавиши **CTRL**+**F6** на клавиатуре.

### <a name="use-serial-console-with-a-screen-reader"></a>Использование последовательной консоли с программой чтения с экрана
Последовательная консоль имеет встроенную поддержку чтения экрана. Навигация с помощью включенного средства чтения экрана дает возможность чтения текста вслух с помощью выбранной кнопки.

## <a name="known-issues"></a>Известные проблемы
Нам известно о некоторых проблемах с Серийной консолью и операционной системой виртуальной машины. Ниже приведен список этих проблем и действий по их устранению для виртуальных машин Linux. Эти проблемы и способы их устранения относятся как к виртуальным машинам, так и к экземплярам масштабируемых наборов виртуальных машин. Если здесь нет нужной ошибки, ознакомьтесь со статьей о [распространенных ошибках в Серийной консоли](./serial-console-errors.md).

Проблема                           |   Меры по снижению риска
:---------------------------------|:--------------------------------------------|
Нажатие клавиши **ВВОД** после заголовка соединения не вызывает появления запроса на вход в систему. | Возможно, GRUB настроен неправильно. Выполните следующие команды: `grub2-mkconfig -o /etc/grub2-efi.cfg` и/или `grub2-mkconfig -o /etc/grub2.cfg` . Дополнительные сведения см. в статье [Нажатие клавиши ВВОД не приводит ни к каким результатам](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Эта проблема может возникать, если вы используете пользовательскую виртуальную машину, зафиксированное устройство или конфигурацию GRUB, которая заставляет Linux подключаться к последовательному порту.
Текст последовательной консоли только занимает часть размера экрана (часто после использования текстового редактора). | Последовательные консоли не поддерживают согласование с размером окна ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)). Это означает, что сигнал обновления размера экрана SIGWINCH не отправляется и виртуальная машина не может определить размер окна терминала. Установите xterm или другую подобную служебную программу, содержащую команду `resize`, а затем выполните команду `resize`.
Вставка длинных строк не работает. | Последовательная консоль ограничивает длину строк, которые можно вставить в окно терминала, до 2048 знаков, чтобы избежать перегрузки пропускной способности серийного порта.
Неустойчивый ввод с клавиатуры в образах SLES BYOS. Ввод с клавиатуры осуществляется только в течение нерегулярного распознавания. | Это проблема с пакетом Плимаус. Плимаус не следует запускать в Azure, так как не требуется экран-заставка, и Плимаус мешает платформе использовать последовательную консоль. Удалите Плимаус с `sudo zypper remove plymouth` и перезагрузите компьютер. Кроме того, измените строку ядра для конфигурации GRUB, добавив `plymouth.enable=0` в конец строки. Это можно сделать путем [изменения загрузочной записи во время загрузки](./serial-console-grub-single-user-mode.md#single-user-mode-in-suse-sles)или путем изменения GRUB_CMDLINE_LINUX строки в `/etc/default/grub` , перестроения GRUB с помощью `grub2-mkconfig -o /boot/grub2/grub.cfg` и последующей перезагрузки.


## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**В. Как отправить отзыв?**

A. Оставьте отзыв, разместив запись на GitHub по адресу https://aka.ms/serialconsolefeedback. Вы также можете (менее предпочтительно) отправить отзыв по адресу azserialhelp@microsoft.com или в категории виртуальной машины на сайте https://feedback.azure.com.

**В. Поддерживает ли последовательная консоль копирование и вставку?**

A. Да. Чтобы копировать и вставить текст в окно терминала, используйте клавиши **CTRL**+**SHIFT**+**C** и **CTRL**+**SHIFT**+**V**.

**Формате. Можно ли использовать последовательную консоль вместо SSH-подключения?**

A. Хотя это может показаться технически возможным, последовательная консоль предназначена для использования в качестве средства устранения неполадок в случаях, когда подключение по протоколу SSH не возможно. Мы не рекомендуем использовать последовательную консоль в качестве замещения SSH по таким причинам:

- Последовательная консоль не имеет такую пропускную способность, как SSH. Это текстовое соединение, поэтому в последовательной консоли сложнее взаимодействовать с графическим интерфейсом пользователя (GUI).
- Доступ к последовательной консоли в данный момент можно получить только с помощью имени пользователя и пароля. Ключи SSH более безопасны, чем комбинации имени пользователя и пароля, поэтому с точки зрения безопасности входа рекомендуется запускать SSH через последовательную консоль.

**Формате. Кто может включать или отключать последовательную консоль для моей подписки?**

A. Чтобы включить или отключить последовательную консоль на уровне подписки, у вас должно быть разрешение на запись в этой подписке. Разрешение на запись есть у ролей администратора и владельца. У пользовательских ролей также может быть разрешение на запись.

**Формате. Кто может получить доступ к последовательной консоли для моей виртуальной машины или масштабируемого набора виртуальных машин?**

A. Чтобы получить доступ к последовательной консоли, необходимо иметь роль участника виртуальной машины или более высокий уровень для ВИРТУАЛЬНЫХ машин или масштабируемых наборов виртуальных машин.

**В. На последовательной консоли ничего не отображается, что делать?**

A. Возможно, ваш образ неправильно настроен для доступа к последовательной консоли. Дополнительные сведения о настройке образа для включения последовательной консоли см. в разделе [Доступность дистрибутива Linux для последовательной консоли](#serial-console-linux-distribution-availability).

**В. Доступна ли последовательная консоль для масштабируемых наборов виртуальных машин?**

A. Да, доступна. Ознакомьтесь со сведениями в разделе [Серийная консоль для Масштабируемых наборов виртуальных машин](serial-console-overview.md#serial-console-for-virtual-machine-scale-sets).

**Формате. Если настроить ВИРТУАЛЬную машину или масштабируемый набор виртуальных машин, используя только проверку подлинности с помощью ключа SSH, можно ли по-прежнему использовать последовательную консоль для подключения к экземпляру масштабируемого набора виртуальных машин или виртуальной машины?**

A. Да. Так как для последовательной консоли не требуются ключи SSH, все, что вам нужно, — это настроить имя пользователя и пароль. Это можно сделать, выбрав **Сброс пароля** на портале Azure и используя эти учетные данные для входа в последовательную консоль.

## <a name="next-steps"></a>Дальнейшие действия
* Используйте последовательную консоль для [доступа к GRUB и однопользовательскому режиму](serial-console-grub-single-user-mode.md).
* Используйте последовательную консоль для [вызовов SysRq и NMI](serial-console-nmi-sysrq.md).
* Узнайте, как использовать последовательное консоль для [включения GRUB в различных дистрибутивов](serial-console-grub-proactive-configuration.md)
* Последовательная консоль также доступна для [виртуальных машин Windows](./serial-console-windows.md).
* См. дополнительные сведения в статье [Устранение неполадок виртуальных машин Windows в Azure с использованием диагностики загрузки](boot-diagnostics.md).