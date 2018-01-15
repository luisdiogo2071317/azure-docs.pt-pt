---
title: Roda os segredos na pilha do Azure | Microsoft Docs
description: Saiba como rodar os segredos na pilha do Azure.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: 0a4118a8927e4261fafa307af5b9c29623ce5c3f
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Roda os segredos na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Atualize as palavras-passe para os componentes de pilha do Azure a uma cadência regular.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Atualizar as palavras-passe para o controlador de gestão de placas base (BMC)

Os controladores de gestão de placas base (BMC) monitorizar o estado dos seus servidores físico. As especificações e instruções sobre como atualizar a palavra-passe do BMC variam com base no seu fornecedor de hardware de fabricante de equipamento original (OEM).

1. Atualize o BMC em servidores físicos a pilha do Azure ao seguir as instruções do OEM. A palavra-passe para cada BMC no seu ambiente tem de ser o mesmo.
2. Abra um ponto final com privilégios em sessões de pilha do Azure. Para instruções, consulte [utilizando o ponto final com privilégios na pilha de Azure](azure-stack-privileged-endpoint.md).
3. Depois do PowerShell linha foi alterada para **[endereço IP ou ERCS VM nome]: PS >** ou **[azs ercs01]: PS >**, consoante o ambiente, execute `Set-BmcPassword` executando `invoke-command`. A variável de sessão do ponto final com privilégios de passar como parâmetro.  
Por exemplo:
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a segurança e a pilha do Azure, consulte [postura de segurança do Azure pilha infraestrutura](azure-stack-security-foundations.md).