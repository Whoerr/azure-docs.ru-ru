---
title: Исследование рисков в защите идентификации Azure Active Directory
description: Узнайте, как изучать действия пользователей, обнаружения и операции входа, которые могут представлять риск в Защите идентификации Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: be72c2152bdb8e1155d2dd29547f93ba3605d462
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2020
ms.locfileid: "95251099"
---
# <a name="how-to-investigate-risk"></a>Практическое руководство. Анализ риска

Защита идентификации предоставляет организациям три отчета, которые они могут использовать для исследования рисков идентификации в их среде. Это отчеты о **рискованных пользователях**, **рискованных входах** и **обнаружении рисков**. Исследование событий является ключом к более глубокому пониманию и идентификации слабых точек в стратегии безопасности.

Все три отчета позволяют скачивать события в формате CSV для дальнейшего анализа за пределами портала Azure. В отчете о рискованных пользователях и событиях входа в систему можно загружать последние 2500 записей, а в отчете об обнаружении рисков — последние 5000 записей.

Организации могут воспользоваться преимуществами интеграции API Microsoft Graph для агрегирования данных с другими источниками, к которым у них может быть доступ в качестве организации.

Три отчета расположены по пути **портал Azure** > **Azure Active Directory** > **Безопасность**.

## <a name="navigating-the-reports"></a>Навигация по отчету

Каждый отчет содержит список всех обнаружений за период, показанный в верхней части отчета. Каждый отчет допускает добавление или удаление столбцов в зависимости от предпочтений администратора. Администраторы могут выбрать загрузку данных в формате CSV или JSON. С помощью фильтров в верхней части отчета можно фильтровать данные в отчете.

При выборе отдельных записей в верхней части отчета могут быть включены дополнительные записи, такие как возможность подтвердить вход в систему как скомпрометированный или надежный, подтвердить, что пользователь скомпрометирован, или отклонить пользовательский риск.

Выбор отдельных записей расширяет окно сведений под обнаружением. Детальный вид позволяет администраторам исследовать и выполнять действия при каждом обнаружении. 

![Пример отчета о защите идентификации, отображающий рискованные входы и сведения](./media/howto-identity-protection-investigate-risk/identity-protection-risky-sign-ins-report.png)

## <a name="risky-users"></a>Пользователи, выполняющие рискованные действия

Информация, доступная в отчете о пользователях, выполняющих рискованные действия, поможет администратору найти следующие сведения:

- какие пользователи являются рискованными, были ли риски исправлены или отклонены?
- Детальные сведения об обнаружении
- Все события рисков при входе
- Журнал рисков
 
Затем администраторы могут предпринять действия с этими событиями. Администраторы могут:

- Сбросить пароль пользователя
- Подтвердить компрометацию пользователя
- Параметр "Закрыть уведомление о риске для пользователя"
- Блокировать вход для выбранного пользователя
- Изучать с помощью Azure ATP

## <a name="risky-sign-ins"></a>Вход, представляющий риск

Отчет о событиях входа с потенциальными рисками содержит фильтруемые данные за последние 30 дней (1 месяц).

Информация, доступная в отчете о пользователях, выполняющих рискованные входы в систему, поможет администратору найти следующие сведения:

- какие операции входа считаются рискованными, скомпрометированными, подтверждены как надежные, отклонены или исправлены.
- Уровни риска в режиме реального времени и накопленного риска, связанные с попытками входа.
- Сработавшие типы обнаружения
- Применение политик условного доступа
- Сведения о многофакторной проверке подлинности (MFA)
- Сведения об устройстве
- Сведения о приложении
- Сведения о расположении

Затем администраторы могут предпринять действия с этими событиями. Администраторы могут:

- Подтвердить компрометацию входа
- Подтвердить безопасный вход

> [!NOTE] 
> Служба защиты идентификации оценивает степень риска для всех потоков аутентификации, как интерактивных, так и не интерактивных. Однако отчет об операциях входа содержит только интерактивные операции входа. Могут отображаться рискованные входы в систему, которые произошли в неинтерактивных входах, но вход не будет отображаться в отчете о входе в Azure AD.

## <a name="risk-detections"></a>Обнаружение рисков

Отчет об обнаружении потенциальных рисков содержит фильтруемые данные за последние 90 дней (3 месяц).

Информация, доступная в отчете об обнаружении рисков, поможет администратору найти следующие сведения:

- сведения о каждом обнаружении рисков, включая тип.
- Другие риски, активированные в то же время
- Расположение попытки входа
- Ссылка на дополнительные сведения из Microsoft Cloud App Security (МКАС).

Затем администраторы могут вернуться к отчету о рисках или о входах пользователей для выполнения действий на основе собранных данных.

> [!NOTE] 
> Наша система может обнаружить, что событие риска, которое участвовало в оценке риска для пользователей риска, было ложным положительным числом, или риск для пользователя был исправлен с помощью принудительного применения политики, например для выполнения запроса MFA или безопасного изменения пароля. Поэтому наша система откроет состояние риска и сведения о риске "AI подтвердил вход в систему" будут отображаться, и он больше не будет участвовать в риске пользователя. 


## <a name="next-steps"></a>Дальнейшие действия

- [Identity Protection policies](concept-identity-protection-policies.md) (Политики защиты идентификации)

- [Включение политик входа и риска для пользователей](howto-identity-protection-configure-risk-policies.md)
