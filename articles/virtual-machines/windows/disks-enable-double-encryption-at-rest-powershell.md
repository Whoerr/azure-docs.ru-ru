---
title: Azure PowerShell-включить двойное шифрование на управляемых дисках RESTful
description: Включите двойное шифрование неактивных данных для управляемых дисков с помощью Azure PowerShell.
author: roygara
ms.date: 08/24/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 9f0642d3421e775d1c6930b672b37643a961178f
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736294"
---
# <a name="use-the-azure-powershell-module-to-enable-double-encryption-at-rest-for-managed-disks"></a>Использование модуля Azure PowerShell для включения двойного шифрования неактивных компонентов для управляемых дисков

Хранилище дисков Azure поддерживает двойное шифрование при хранении для управляемых дисков. Концептуальные сведения о двойном шифровании неактивных данных и других типах шифрования дисков см. в разделе [двойное шифрование при](../disk-encryption.md#double-encryption-at-rest) хранении статьи о шифровании диска.

## <a name="prerequisites"></a>Предварительные условия

Установите последнюю [версию Azure PowerShell](/powershell/azure/install-az-ps)и войдите в учетную запись Azure с помощью [Connect-азаккаунт](/powershell/module/az.accounts/connect-azaccount).

## <a name="getting-started"></a>Начало работы

1. Создайте экземпляр Azure Key Vault и ключ шифрования.

    При создании экземпляра Key Vault необходимо включить обратимое удаление и защиту от удаления. Обратимое удаление гарантирует, что Key Vault будет хранить удаленный ключ в течение заданного срока (по умолчанию 90 дней). Защита от удаления гарантирует, что удаленный ключ нельзя удалить навсегда до истечения срока хранения. Эти параметры защищают от потери данных из-за случайного удаления. Эти параметры являются обязательными при использовании Key Vault для шифрования управляемых дисков.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westus2"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.  Создайте Дискенкриптионсет с encryptionType, установленным как Енкриптионатрествисплатформандкустомеркэйс. Используйте API версии **2020-05-01** в шаблоне Azure Resource Manager (ARM). 
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/DoubleEncryption/CreateDiskEncryptionSetForDoubleEncryption.json" `
    -diskEncryptionSetName $diskEncryptionSetName `
    -keyVaultId $keyVault.ResourceId `
    -keyVaultKeyUrl $key.Key.Kid `
    -encryptionType "EncryptionAtRestWithPlatformAndCustomerKeys" `
    -region $LocationName
    ```

1. Предоставьте ресурсу DiskEncryptionSet доступ к хранилищу ключей.

    > [!NOTE]
    > Создание удостоверения DiskEncryptionSet в Azure Active Directory может занять несколько минут. Если при выполнении следующей команды появляется сообщение об ошибке вида "Не удается найти объект Active Directory", подождите несколько минут и повторите попытку.

    ```powershell  
    $des=Get-AzDiskEncryptionSet -name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
    ```

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы создали и настроили эти ресурсы, их можно использовать для защиты управляемых дисков. Следующие ссылки содержат примеры сценариев, каждый из которых имеет соответствующий сценарий, который можно использовать для защиты управляемых дисков.

- [Azure PowerShell — включить управляемые клиентом ключи с помощью управляемых дисков с шифрованием на стороне сервера](disks-enable-customer-managed-keys-powershell.md)
- [Примеры шаблонов Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)
