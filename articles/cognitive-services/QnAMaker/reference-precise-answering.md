---
title: Точный ответ с использованием обнаружения диапазона ответов — QnA Maker
description: Ознакомьтесь с точной функцией ответа, доступной в QnA Maker управляемых.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/09/2020
ms.openlocfilehash: 4f64bab698cb87e26fa4fd1587c4269acf99fa59
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384110"
---
# <a name="precise-answering"></a>Точный ответ

Функция точного ответа позволяет получить точный ответ от лучших ответов на кандидаты, имеющихся в базе знаний для любого пользовательского запроса. Эта функция использует модель глубокого обучения в среде выполнения, которая понимает цель пользовательского запроса и обнаруживает точный короткий ответ от пути ответа, если в ответ на вопрос существует небольшой ответ. 

Эта функция включена по умолчанию в области тестирования, поэтому можно проверить функциональные возможности, характерные для вашего сценария. Эта функция чрезвычайно полезна как для разработчиков содержимого, так и для конечных пользователей. Теперь разработчикам содержимого не нужно вручную выполнять определенные пары QnA для каждого факта, присутствующего в базе знаний, и конечному пользователю не нужно просматривать весь ответ, полученный от службы, чтобы найти фактический факт, который отвечает на запрос пользователя. 

## <a name="precise-answering-on-qna-maker-portal"></a>Точный ответ на портале QnA Maker

На портале QnA Maker при открытии тестовой панели вы увидите параметр для **вывода короткого ответа** вверху. Этот параметр будет выбран по умолчанию. При вводе запроса в области тестирования вы увидите короткий ответ, а также ответ, если в ответе есть небольшой ответ. 
 
![Управляемая область теста с поддержкой](../QnAMaker/media/conversational-context/test-pane-with-managed.png)

Можно отменить выбор параметра **Отображать короткий ответ** , если на панели тестирования нужно просмотреть только сведения о ответе. 

Служба также возвращает оценку достоверности точного ответа в виде **оценки интервала ответа** , которую можно проверить, выбрав параметр **проверить** прямо под запросом в области тестирования.

![Оценка диапазона управляемого ответа](../QnAMaker/media/conversational-context/managed-answer-span-score.png)

## <a name="publishing-a-qna-maker-bot"></a>Публикация QnA Maker Bot

При публикации программы-робота в приложении по умолчанию устанавливается точный интерфейс с включенным ответом, где вы увидите короткий ответ и ответ. Пользователь имеет возможность выбрать другие возможности, обновив шаблон с помощью службы приложений Ебот. 

## <a name="language-support"></a>Поддержка языков

В настоящее время Точная функция ответа поддерживается только для английского языка.
