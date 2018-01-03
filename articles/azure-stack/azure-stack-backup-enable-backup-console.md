---
title: "Ativar a cópia de segurança para a pilha do Azure no portal de administração da | Microsoft Docs"
description: "Ative o infraestrutura novamente serviço através do portal de administração para que a pilha do Azure pode ser restaurada se ocorrer uma falha."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Ativar a cópia de segurança para a pilha do Azure no portal de administração

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Ative o infraestrutura novamente serviço através do portal de administração para que a pilha do Azure pode gerar cópias de segurança. Pode utilizar estes efetua uma cópia ups para restaurar o seu ambiente se ocorrer uma falha.

## <a name="enable-backup"></a>Ativar a cópia de segurança

1. Abra o portal de administração do Azure pilha em [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolha **configuração** no **cópia de segurança da infraestrutura** painel.

    ![Pilha do Azure - definições de controlador de cópia de segurança](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Tem de utilizar uma cadeia de convenção de Nomenclatura Universal (UNC) para o caminho de uma partilha de ficheiros alojada num dispositivo separado. Uma cadeia em UNC Especifica a localização dos recursos, tais como ficheiros partilhados ou dispositivos. Para o serviço, pode utilizar um endereço IP. Para garantir a disponibilidade dos dados de cópia de segurança em caso de desastre, o dispositivo deve estar num local separado.
    > [!Note]  
    > Pode utilizar um FQDN, em vez de IP se o seu ambiente suportar resolução do nome de rede de infraestrutura de pilha do Azure para o seu ambiente empresarial.
4. Tipo de **Username** com o domínio e o nome de utilizador. Por exemplo, `Contoso\administrator`.
5. Tipo de **palavra-passe** para o utilizador.
5. Escreva a palavra-passe novamente a **Confirmar palavra-passe**.
6. Fornecer uma chave pré-partilhada no **chave de encriptação** caixa. Ficheiros de cópia de segurança estão encriptados com esta chave. Certifique-se armazenar esta chave numa localização segura. Depois de definir esta chave pela primeira vez ou rodar a chave no futuro, não é possível ver esta chave desta interface. Para obter mais instruções gerar uma chave pré-partilhada, siga os scripts em [ativar a cópia de segurança de pilha do Azure com o PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Selecione **OK** para guardar as definições de cópia de segurança do controlador.

Para executar uma cópia de segurança, tem de transferir as ferramentas de pilha do Azure e, em seguida, execute o cmdlet do PowerShell **início AzSBackup** no nó de administração a pilha do Azure. Para obter mais informações, consulte [cópia de segurança do Azure pilha](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Passos Seguintes

 - Saiba como executar uma cópia de segurança, consulte [cópia de segurança do Azure pilha](azure-stack-backup-back-up-azure-stack.md ).
- Saiba como verificar que executou a cópia de segurança, consulte [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md ).
