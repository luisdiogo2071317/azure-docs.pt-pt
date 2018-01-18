---
title: "Cópia de segurança do Azure pilha | Microsoft Docs"
description: "Efetue uma cópia de segurança a pedido na pilha do Azure com cópia de segurança no local."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 955b286967ca2bc8450e8988ec16c6a5c352aa8a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="back-up-azure-stack"></a>Cópia de segurança de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Efetue uma cópia de segurança a pedido na pilha do Azure com cópia de segurança no local. Se precisar de ativar o serviço de cópia de segurança da infraestrutura, consulte [ativar a cópia de segurança do Azure pilha do portal de administração do](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Ferramentas de pilha do Azure contêm o **início AzSBackup** cmdlet. Para obter instruções sobre como instalar as ferramentas, consulte [começar a trabalhar com o PowerShell na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

## <a name="start-azure-stack-backup"></a>Iniciar a cópia de segurança de pilha do Azure

Abra o Windows PowerShell com uma linha de comandos elevada do ambiente de gestão do operador e execute os seguintes comandos:

```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1

    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Confirme a cópia de segurança foi concluída no portal de administração do

1. Abra o portal de administração do Azure pilha em [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolha **configuração** no **cópia de segurança da infraestrutura** painel.
3. Localizar o **nome** e **data concluída** da cópia de segurança no **cópias de segurança disponíveis** lista.
4. Certifique-se a **estado** é **com êxito**.

Também pode confirmar a cópia de segurança de portal de administração do. Navegue para`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o fluxo de trabalho para recuperar a partir de um evento de perda de dados. Consulte [recuperar a partir de perda catastrófica de dados](azure-stack-backup-recover-data.md).
