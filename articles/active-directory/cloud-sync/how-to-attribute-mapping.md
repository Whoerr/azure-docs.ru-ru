---
title: Azure AD Connect редактор атрибутов облачной синхронизации
description: В этой статье описывается, как использовать редактор атрибутов.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2021
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6d2adbd0fe0715cb22ac158d1804f53384f8b94
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2021
ms.locfileid: "98682111"
---
# <a name="azure-ad-connect-cloud-sync-attribute-mapping"></a>Azure AD Connect сопоставление атрибута облачной синхронизации

Azure AD Connect облачная синхронизация предоставила новую функцию, которая позволит легко сопоставлять атрибуты локальных объектов User/Group и объектов в Azure AD.  Эта функция добавлена в конфигурацию облачной синхронизации.

Сопоставление атрибутов по умолчанию можно настроить в соответствии с потребностями вашего бизнеса. Поэтому можно изменить или удалить существующие сопоставления атрибутов или создать новые.  Список синхронизируемых атрибутов см. в разделе [Синхронизируемые атрибуты](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md).

## <a name="understanding-attribute-mapping-types"></a>Основные сведения о типах сопоставления атрибутов
С помощью сопоставления атрибутов можно управлять заполнением атрибутов в Azure AD.
Поддерживаются четыре типа сопоставлений:

- **Direct** — целевой атрибут заполняется значением атрибута связанного объекта в Active Directory.
- **Постоянное** — целевой атрибут заполняется определенной строкой, указанной вами.
- **Выражение** — целевой атрибут заполняется на основе результата выражения, похожего на сценарий.
  Дополнительные сведения см. в разделе [Написание выражений для сопоставлений атрибутов](reference-expressions.md).
- **Нет** — целевой атрибут остается без изменений. Тем не менее, если целевой атрибут пуст, он заполняется указанным вами значением по умолчанию.

Кроме этих четырех основных типов, в сопоставлениях настраиваемых атрибутов также поддерживается назначение значений **по умолчанию**. Назначение значений по умолчанию гарантирует, что целевой атрибут заполняется значением, если нет значения ни в Azure AD, ни в целевом объекте. В наиболее распространенной конфигурации это поле остается пустым.

## <a name="understanding-attribute-mapping-properties"></a>Основные сведения о свойствах сопоставления атрибутов

В предыдущем разделе мы уже рассмотрели свойство типа для сопоставления атрибутов.
Наряду с этим свойством сопоставления атрибутов также поддерживают следующие атрибуты:

- **Исходный атрибут** — атрибут пользователя из исходной системы (пример: Active Directory).
- **Целевой атрибут** — атрибут пользователя в целевой системе (пример: Azure Active Directory).
- **Значение по умолчанию для NULL (необязательно)** — значение, которое будет передано в целевую систему, если исходный атрибут имеет значение NULL. Это значение будет подготовлено только при создании пользователя. При обновлении существующего пользователя параметр "Значение по умолчанию для NULL (необязательно)" не подготавливается.  
- **Применить это сопоставление**
  - **Всегда** — применение этого сопоставления как при создании, так и при обновлении пользователей.
  - **Только при создании** — применение этого сопоставления только при создании пользователей.

> [!NOTE]
> В этом документе описано, как использовать портал Azure для отображения атрибутов.  Сведения об использовании Graph см. в разделе [преобразования](how-to-transformation.md) .

## <a name="using-attribute-mapping"></a>Использование сопоставления атрибутов

Чтобы использовать новую функцию, выполните следующие действия.

1.  На портале Azure выберите **Azure Active Directory**.
2.  Выберите **Azure AD Connect**.
3.  Выберите **Управление облачной синхронизацией**.

    ![Управление подготовкой](media/how-to-install/install-6.png)

4. В разделе **Конфигурация** выберите свою конфигурацию.
5. Выберите **щелкните, чтобы изменить сопоставления**.  Откроется экран сопоставление атрибутов.

    ![Добавление атрибутов](media/how-to-attribute-mapping/mapping-6.png)

6.  Нажмите кнопку **Добавить атрибут**.

    ![Тип сопоставления](media/how-to-attribute-mapping/mapping-1.png)

7. Выберите **тип сопоставления**.  В этом примере используется выражение.
8.  Введите выражение в поле.  Для этого примера мы используем: `Replace([mail], "@contoso.com", , ,"", ,).`
9.  Введите целевой атрибут.  В этом примере мы используем ExtensionAttribute15.
10. Выберите, когда следует применить это, а затем нажмите кнопку **Применить** .

    ![Изменить сопоставления](media/how-to-attribute-mapping/mapping-2a.png)

11. Вернитесь на экран сопоставление атрибутов, вы увидите новое сопоставление атрибутов.  
12. Нажмите кнопку **сохранить схему**.

    ![Сохранить схему](media/how-to-attribute-mapping/mapping-3.png)

## <a name="test-your-attribute-mapping"></a>Тестирование сопоставления атрибутов

Чтобы проверить сопоставление атрибутов, можно использовать [подготовку по требованию](how-to-on-demand-provision.md).  Из 

1. На портале Azure выберите **Azure Active Directory**.
2. Выберите **Azure AD Connect**.
3. Выберите **Управление инициализацией**.
4. В разделе **Конфигурация** выберите свою конфигурацию.
5. В разделе **Проверка** нажмите кнопку **подготавливать пользователя** . 
6. На экране подготовки по требованию.  Введите **различающееся имя** пользователя или группы и нажмите кнопку " **подготавливать** ".  
7. После завершения работы вы увидите экран Success (успешно) и 4 зеленые флажки, указывающие, что оно было успешно подготовлено.  

    ![Успешное завершение подготовки](media/how-to-attribute-mapping/mapping-4.png)

8. В разделе **выполнение действия** щелкните **Просмотр сведений**.  Справа должен отобразиться новый атрибут SYNCHRONIZED и применено выражение.

  ![Выполнить действие](media/how-to-attribute-mapping/mapping-5.png)

## <a name="next-steps"></a>Дальнейшие шаги

- [Что представляет собой облачная синхронизация Azure AD Connect?](what-is-cloud-sync.md)
- [Запись выражений для сопоставления атрибутов](reference-expressions.md)
- [Синхронизированные атрибуты](../hybrid/reference-connect-sync-attributes-synchronized.md?context=azure%2factive-directory%2fcloud-provisioning%2fcontext%2fcp-context/hybrid/reference-connect-sync-attributes-synchronized.md)
