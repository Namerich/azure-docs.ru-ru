---
title: Настройка правил и управление оповещениями
description: Описание настройки правил и управления оповещениями в Фармбеатс
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 5e6a5d414c341f482c3fddf95a2f8bb8e55a3ca2
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168551"
---
# <a name="configure-rules-and-manage-alerts"></a>Настройка правил и управление оповещениями

Azure Фармбеатс позволяет создавать правила на основе бизнес-логики, а также данные датчиков, которые поступают из датчиков и устройств, развернутых в ферме. Правила активируют предупреждения в системе, когда значения датчика пересекаются пороговое значение. Просмотр и анализ оповещений, созданных после пороговых значений, позволяет быстро реагировать на любые проблемы и получать необходимые решения.

## <a name="create-rule"></a>Создание правила

1. На домашней странице перейдите к разделу **правила**.
2. Выберите **новое правило**. Откроется окно Новое правило.

    ![Снимок экрана, на котором выделена кнопка "создать правило" и раздел "новое правило".](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-1.png)

3. Введите **имя правила** и **Описание правила** , а затем выберите ферму из раскрывающегося меню **Выбор фермы** .
4. Введите имя фермы, чтобы выбрать раздел "ферма и **условия** " в том же окне.  

    ![Снимок экрана, посвященный разделу условий.](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-condition-1.png)

5. В поле **условия**введите значения для **меры**, **operator** и **value**.
6. В раскрывающемся меню **мера** введите имя меры.
7. Выберите **+ Добавить условие** , чтобы добавить дополнительные условия в правило.
8. Выберите **уровень серьезности**.
9. В меню **действие**включите переключатель с **включенной электронной почтой** , чтобы включить оповещения по электронной почте.

    ![Снимок экрана, на котором показан параметр "Электронная почта включена".](./media/configure-rules-and-alerts-in-azure-farmbeats/new-rule-email-1.png)

10. Введите **адреса электронной почты** , на которые вы хотите отправить оповещение по электронной почте, а также **тему сообщения электронной** почты и **Дополнительные примечания**.  
11. В поле **состояние правила**установите переключатель в положение **включено** , чтобы включить или отключить правило.
    Вы можете просмотреть число устройств, на которые будет повлиять данное правило.
12. Нажмите кнопку **Применить** , чтобы создать правило.

## <a name="view-rule"></a>Просмотр правила

На странице **ферма** отображается список доступных правил. Выберите **имя правила**. В окне отображаются следующие сведения, применимые к выбранному правилу.
 - Имя правила
 - Ссылка на ферму, с которой связано правило
 - Дата создания
 - Дата последнего обновления
 - Степень серьезности
 - Состояние правила
 - Список условий  
 - Число устройств, затронутых правилом

    ![Снимок экрана, на котором отображается экран сведений о правиле.](./media/configure-rules-and-alerts-in-azure-farmbeats/view-rule-1.png)

## <a name="edit-rule"></a>Изменение правила

Чтобы изменить правило, выполните следующие действия.

1. На домашней странице в меню навигации слева выберите **правила** .
   Откроется окно правила.
2. Выберите правило, для которого необходимо изменить значение.

    ![Снимок экрана, на котором отображается выбранное правило.](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-action-bar-1.png)

3. Выберите **изменить** на панели действий, откроется окно **Изменение правила** .

    ![Снимок экрана, на котором показан экран изменения правила.](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-one-1.png)

4. Измените **имя правила**и **Описание правила** , а затем выберите ферму в раскрывающемся меню **Выбрать ферму** .
5. Введите имя фермы, чтобы выбрать ферму, и **условия** отображаются в том же окне.  
6. В **условиях**, измените **меру**, **оператор** и **значение**.
7. В раскрывающемся меню **мера** введите имя меры.
8. Выберите **+ Добавить условие** , чтобы добавить или изменить условия правил.

    ![Снимок экрана, на котором выделяется кнопка "добавить условие".](./media/configure-rules-and-alerts-in-azure-farmbeats/edit-rule-two-1.png)

9.  Выберите **уровень серьезности**.  
10. В меню **действие**включите переключатель с **включенной электронной почтой** , чтобы включить оповещения по электронной почте.
11. Измените **адреса электронной почты** , по которым вы хотите отправить оповещение по электронной почте, а также **тему сообщения электронной** почты и **Дополнительные примечания**.  
12. В поле **состояние правила**установите переключатель в положение **включено** , чтобы включить или отключить правило.
Вы можете просмотреть число устройств, которые будут затронуты этим правилом.
13. Нажмите кнопку **Применить** , чтобы изменить правило.

## <a name="change-rule-status"></a>Изменить состояние правила

Чтобы изменить состояние правила, выполните следующие действия.

1. На домашней странице в меню навигации слева выберите **правила** . Откроется окно правила.
2. Выберите правило, состояние которого необходимо изменить.

    ![Снимок экрана, на котором показана кнопка "изменить состояние".](./media/configure-rules-and-alerts-in-azure-farmbeats/change-status-rule-action-bar-1.png)

3. Выберите **изменить состояние** на панели действий. Отобразится окно **состояние изменения** .

    ![Снимок экрана, на котором отображается экран состояния изменения.](./media/configure-rules-and-alerts-in-azure-farmbeats/rule-change-status-1.png)

3. Измените состояние правила с помощью переключателя **изменить состояние** .
   Вы можете просмотреть число устройств, на которые будет повлиять данное правило.
4. Нажмите кнопку **Применить** , чтобы изменить состояние правила.

## <a name="delete-rule"></a>Удаление правила.

Чтобы удалить правило, выполните следующие действия.

1. На домашней странице в меню навигации слева выберите **правила** . Откроется окно правила.
2. Выберите правило, для которого необходимо удалить.

    ![Снимок экрана, на котором выделена кнопка "Удалить".](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-action-bar-1.png)

3. Выберите **Удалить** на панели действий.

    ![Проект FarmBeats](./media/configure-rules-and-alerts-in-azure-farmbeats/delete-rule-1.png)

4. Откроется диалоговое окно **Удаление правила** . Нажмите кнопку **Удалить**.
