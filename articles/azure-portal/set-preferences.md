---
title: Управление параметрами и предпочтениями портала Azure
description: Параметры портал Azure по умолчанию можно изменить в соответствии с собственными предпочтениями. Параметры включают время ожидания неактивного сеанса, представление по умолчанию, режим меню, контрастность, тему, уведомления, язык и региональные форматы
keywords: параметры, время ожидания, язык, региональный
ms.date: 08/05/2020
ms.topic: how-to
ms.openlocfilehash: 591b1ce6c2c069f9c0266588bf32f7303bf49708
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100588800"
---
# <a name="manage-azure-portal-settings-and-preferences"></a>Управление параметрами и предпочтениями портала Azure

Можно изменить параметры портал Azure по умолчанию в соответствии с собственными предпочтениями. Большинство параметров доступны в меню **Параметры** в заголовке глобальной страницы.

![Снимок экрана: значки верхнего колонтитула глобальной страницы с выделенными параметрами](./media/set-preferences/header-settings.png)


## <a name="choose-your-default-subscription"></a>Выберите подписку по умолчанию

Вы можете изменить подписку, которая открывается по умолчанию при входе в портал Azure. Это полезно, если у вас есть первичная подписка, с которой вы работаете, но иногда используете другие. 

:::image type="content" source="media/set-preferences/filter-subscription-default-view.png" alt-text="Фильтрация списка ресурсов по подписке.":::

1. Выберите значок фильтра каталогов и подписок в заголовке глобальной страницы.

1. Выберите подписки, которые будут использоваться в качестве подписок по умолчанию при запуске портала. 

    :::image type="content" source="media/set-preferences/default-directory-subscription-filter.png" alt-text="Выберите подписки, которые будут использоваться в качестве подписок по умолчанию при запуске портала."::: 


## <a name="choose-your-default-view"></a>Выбор представления по умолчанию 

Вы можете изменить страницу, которая открывается по умолчанию при входе в портал Azure.

![Снимок экрана, показывающий параметры портал Azure с выделенным представлением по умолчанию](./media/set-preferences/default-view.png)

- **Домашняя страница** не может быть настроена.  Здесь отображаются ярлыки популярных служб Azure и перечислены наиболее часто используемые ресурсы. Мы также предоставляем полезные ссылки на ресурсы, такие как Microsoft Learn и стратегия Azure.

- Панели мониторинга можно настроить для создания рабочей области, специально предназначенной для вас. Например, можно создать панель мониторинга, которая является задачей проекта, задачи или роли. Если выбрать **панель мониторинга**, то представление по умолчанию будет отправлено на последнюю использовавшуюся панель мониторинга. Дополнительные сведения см. в статье [Создание панелей мониторинга на портале Azure и предоставление общего доступа к ним](azure-portal-dashboards.md).

## <a name="choose-a-portal-menu-mode"></a>Выбор режима меню портала

Режим по умолчанию для меню портала управляет тем, сколько пространства занимает меню портала на странице.

![Снимок экрана, показывающий, как задать режим по умолчанию для меню портала.](./media/set-preferences/menu-mode.png)

- Когда меню портала находится в раскрывающемся **режиме,** оно скрыто до тех пор, пока оно не понадобится. Открыть или закрыть его можно, щелкнув значок меню.

- Если для меню портала выбран **режим закрепленный** , он всегда виден. Вы можете свернуть меню, чтобы увеличить рабочее пространство.

## <a name="choose-a-theme-or-enable-high-contrast"></a>Выберите тему или включите высокую контрастность

Выбранная тема влияет на цвет фона и шрифта, которые отображаются в портал Azure. Можно выбрать одну из четырех предустановленных цветовых тем. Выберите каждый эскиз, чтобы найти наиболее подходящий для вас тему.

Кроме того, можно выбрать одну из тем с высокой контрастностью. Темы с высокой контрастностью упрощают чтение портал Azure для людей с нарушениями зрения. они переопределяют все остальные параметры темы.

![Снимок экрана, показывающий параметры портал Azure с выделенными темами](./media/set-preferences/theme.png)

## <a name="enable-or-disable-pop-up-notifications"></a>Включение или отключение всплывающих уведомлений

Уведомления — это системные сообщения, связанные с текущим сеансом. Они предоставляют сведения, такие как текущий баланс кредита, время доступности только что созданных ресурсов или подтверждение последнего действия. Если всплывающие уведомления включены, сообщения кратко отображаются в верхнем углу экрана. 

Чтобы включить или отключить всплывающие уведомления, установите или снимите флажок **включить всплывающие уведомления**.

![Снимок экрана, показывающий параметры портал Azure с выделенными всплывающими уведомлениями](./media/set-preferences/popup-notifications.png)

Чтобы прочитать все уведомления, полученные во время текущего сеанса, выберите **уведомления** из глобального заголовка.

![Снимок экрана, показывающий портал Azure глобальный заголовок с выделенными уведомлениями](./media/set-preferences/read-notifications.png)

Если вы хотите читать уведомления из предыдущих сеансов, найдите события в журнале действий. Дополнительные сведения см. [в разделе Просмотр журнала действий](../azure-monitor/essentials/activity-log.md#view-the-activity-log). 

## <a name="change-the-inactivity-timeout-setting"></a>Изменение параметра времени ожидания неактивности

Настройка времени ожидания при неактивном доступе помогает защитить ресурсы от несанкционированного доступа, если вы забыли защитить рабочую станцию. Когда вы простаивает, вы автоматически выйдете из сеанса портал Azure. В качестве отдельного пользователя можно изменить настройку времени ожидания. Если вы являетесь администратором, вы можете настроить его на уровне каталога для всех пользователей в каталоге.

### <a name="change-your-individual-timeout-setting-user"></a>Изменение параметра индивидуального времени ожидания (пользователь)

Выберите раскрывающийся список в разделе **выйти, если неактивен**. Выберите продолжительность выхода из сеанса портал Azure в случае бездействия.

![Снимок экрана: параметры портала с выделенными неактивными параметрами времени ожидания](./media/set-preferences/inactive-signout-user.png)

Изменение сохраняется автоматически. Если вы простаиваете, ваш сеанс портал Azure выйдет по истечении установленной вами длительности.

Если администратор включил политику истечения времени ожидания бездействия, вы по-прежнему можете задать собственное значение, если оно меньше параметра уровня каталога. Установите флажок **переопределить политику времени ожидания бездействия в каталоге**, а затем задайте интервал времени.

![Снимок экрана с параметрами портала с переопределением выделенного параметра политики времени ожидания бездействия в каталоге](./media/set-preferences/inactive-signout-override.png)

### <a name="change-the-directory-timeout-setting-admin"></a>Изменение параметра времени ожидания в каталоге (администратор)

Администраторы в [роли глобального администратора](../active-directory/roles/permissions-reference.md#global-administrator) могут принудительно применить максимальное время простоя до выхода из сеанса. Параметр времени ожидания неактивности применяется на уровне каталога. Параметр вступает в силу для новых сеансов. Он не будет немедленно применяться к пользователям, которые уже вошли в. Дополнительные сведения о каталогах см. в разделе Общие сведения о [службах домен Active Directory Services](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

Если вы являетесь глобальным администратором и хотите применить параметр времени ожидания простоя для всех пользователей портал Azure, выполните следующие действия.

1. Выберите текст ссылки **настроить время ожидания на уровне каталога**.

    ![Снимок экрана: параметры портала с выделенным текстом ссылки](./media/set-preferences/settings-admin.png)

1. На странице **Настройка времени ожидания при неактивном уровне каталога** установите флажок **Включить время ожидания простоя уровня каталога для** включения параметра портал Azure.

1. Затем введите **часы** и **минуты** для максимального времени, в течение которого пользователь может бездействовать, прежде чем сеанс автоматически выйдет из сеанса.

1. Нажмите кнопку **Применить**.

    ![Снимок экрана, на котором показана страница для задания времени ожидания бездействия на уровне каталога](./media/set-preferences/configure.png)

Чтобы убедиться, что политика времени ожидания неактивности задана правильно, выберите **уведомления** в заголовке глобальной страницы. Убедитесь, что в списке указано уведомление об успешном выполнении.

![Снимок экрана, показывающий успешное сообщение уведомления об истечении времени ожидания бездействия на уровне каталога](./media/set-preferences/confirmation.png)

## <a name="restore-default-settings"></a>Восстановить настройки по умолчанию

Если вы внесли изменения в параметры портал Azure и хотите их удалить, выберите **восстановить параметры по умолчанию**. Все изменения, внесенные в параметры портала, будут потеряны. Этот параметр не влияет на настройки панели мониторинга.

![Снимок экрана, показывающий восстановление параметров по умолчанию](./media/set-preferences/useful-links-restore-defaults.png)

## <a name="export-user-settings"></a>Экспорт параметров пользователя

Сведения о пользовательских параметрах хранятся в Azure. Вы можете экспортировать следующие данные пользователя:

* Частные панели мониторинга в портал Azure
* Пользовательские параметры, такие как избранные подписки или каталоги, а также каталог последнего входа
* Темы и другие параметры пользовательского портала

Рекомендуется экспортировать и проверить параметры, если планируете их удалить. Перестроение панелей мониторинга или повторных настроек может занять много времени.

Чтобы экспортировать параметры портала, выберите **экспортировать все параметры**.

![Снимок экрана, показывающий экспорт параметров](./media/set-preferences/useful-links-export-settings.png)

При экспорте параметров создается *JSON* -файл, содержащий пользовательские параметры, такие как цветовая тема, избранное и частные панели мониторинга. Из-за динамического характера пользовательских параметров и риска повреждения данных нельзя импортировать параметры из *JSON* -файла.

## <a name="delete-user-settings-and-dashboards"></a>Удаление параметров и панелей мониторинга пользователя

Сведения о пользовательских параметрах хранятся в Azure. Вы можете удалить следующие данные пользователя:

* Частные панели мониторинга в портал Azure
* Пользовательские параметры, такие как избранные подписки или каталоги, а также каталог последнего входа
* Темы и другие параметры пользовательского портала

Рекомендуется экспортировать и проверить параметры перед их удалением. Перестроение панелей мониторинга или повторное выполнение пользовательских параметров может занять много времени.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

Чтобы удалить параметры портала, выберите **удалить все параметры и частные панели мониторинга**.

![Снимок экрана, показывающий удаление параметров](./media/set-preferences/useful-links-delete-settings.png)

## <a name="change-language-and-regional-settings"></a>Изменение языковых и региональных параметров

Существует два параметра, управляющих отображением текста в портал Azure. 
- **Языковой параметр определяет** язык, который отображается для текста в портал Azure. 

- В **региональном формате** определяются способы отображения дат, времени, чисел и валюты.

Чтобы изменить язык, используемый в портал Azure, используйте раскрывающийся список, чтобы выбрать из списка доступных языков.

Выбор регионального формата изменится, чтобы отображались региональные параметры только для выбранного языка. Чтобы изменить этот автоматический выбор, используйте раскрывающийся список, чтобы выбрать нужный региональный формат.

Например, если выбрать английский язык, а затем выбрать США в качестве регионального формата, то в долларах США будет показана денежная единица. Если в качестве языка выбрать английский, а затем в качестве регионального формата выбрать Европа, то в евро будет показана Валюта.

Нажмите кнопку **Применить** , чтобы обновить язык и региональные параметры формата.

   ![Снимок экрана, показывающий параметры языка и регионального формата](./media/set-preferences/language.png)

>[!NOTE]
>Эти языковые и региональные параметры влияют только на портал Azure. Ссылки на документацию, которые открываются в новой вкладке или окне, используют параметры языка браузера для определения языка, который должен отображаться.
>

## <a name="next-steps"></a>Дальнейшие шаги

- [Сочетания клавиш на портале Azure](azure-portal-keyboard-shortcuts.md)
- [Поддерживаемые браузеры и устройства](azure-portal-supported-browsers-devices.md)
- [Добавление, удаление и переупорядочение избранного](azure-portal-add-remove-sort-favorites.md)
- [Создание и совместное использование настраиваемых панелей мониторинга](azure-portal-dashboards.md)
- [Серия видеоинструкций по работе с порталом Azure](azure-portal-video-series.md)
