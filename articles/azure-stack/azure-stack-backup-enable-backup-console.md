---
title: Ativar cópia de segurança para o Azure Stack a partir do portal de administração | Documentos da Microsoft
description: Ative o serviço de cópia de segurança de infraestrutura através do portal de administração para que o Azure Stack pode ser restaurado se ocorrer uma falha.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffgilb
ms.openlocfilehash: 08bce6284b672ae092e2cee3c26140e8c6049a34
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242857"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Ativar cópia de segurança para o Azure Stack a partir do portal de administração
Ative o serviço de cópia de segurança de infraestrutura através do portal de administração para que o Azure Stack pode gerar as cópias de segurança. Pode utilizar estas cópias de segurança para restaurar o seu ambiente na cloud de recuperação na eventualidade de a utilizar [uma falha catastrófica](.\azure-stack-backup-recover-data.md). O objetivo de recuperação na cloud é garantir que seus operadores e os utilizadores podem iniciar sessão no portal do após a conclusão da recuperação. Os utilizadores terão suas assinaturas restauradas, incluindo permissões de acesso baseado em funções e funções, originais planos, ofertas e a computação definidos anteriormente, armazenamento e quotas de rede.

No entanto, o serviço de cópia de segurança de infraestrutura não cópia de segurança de VMs de IaaS, configurações de rede e recursos de armazenamento, como contas de armazenamento, blobs, tabelas, e assim por diante, para que os utilizadores iniciar sessão após a recuperação de nuvem é concluído não verão qualquer um dos anteriormente existente recursos. Plataforma como serviço (PaaS) recursos e os dados são também não backup pelo serviço. 

Os administradores e utilizadores são responsáveis por fazer backup e restaurar os recursos de IaaS e PaaS separadamente dos processos de cópia de segurança da infraestrutura. Para obter informações sobre cópias de segurança recursos de IaaS e PaaS, consulte as seguintes ligações:

- [Máquinas Virtuais](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [Serviço de Aplicações](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Antes de ativar a cópia de segurança através da consola, tem de configurar o serviço de cópia de segurança. Pode configurar o serviço de cópia de segurança com o PowerShell. Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Ativar cópia de segurança

1. Abra o portal de administração do Azure Stack em [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolher **Configuration** no **cópia de segurança da infraestrutura** painel.

    ![O Azure Stack - definições do controlador de cópia de segurança](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Utilize uma cadeia de caracteres de convenção de Nomenclatura Universal (UNC) para o caminho para uma partilha de ficheiros hospedado num dispositivo separado. Uma cadeia de caracteres UNC Especifica a localização de recursos, tais como ficheiros partilhados ou dispositivos. Para o serviço, pode utilizar um endereço IP. Para garantir a disponibilidade dos dados de cópia de segurança após um desastre, o dispositivo deve estar num local separado.
    > [!Note]  
    > Se o ambiente oferecer suporte a resolução do nome de rede de infraestrutura do Azure Stack para seu ambiente empresarial, pode utilizar um FQDN, em vez do IP.
4. Tipo de **nome de utilizador** usando o domínio e nome de utilizador com acesso suficiente para ler e escrever ficheiros. Por exemplo, `Contoso\backupshareuser`.
5. Tipo de **palavra-passe** para o utilizador.
5. Escreva a palavra-passe novamente ao **Confirmar palavra-passe**.
6. Fornecer uma chave pré-partilhada no **chave de encriptação** caixa. Ficheiros de cópia de segurança são encriptados com esta chave. Certifique-se armazenar esta chave numa localização segura. Depois de definir esta chave pela primeira vez ou rotação da chave no futuro, não pode ver esta chave a partir dessa interface. Para obter mais instruções gerar uma chave pré-partilhada, siga os scripts na [ativar cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).
7. Selecione **OK** para guardar as definições de cópia de segurança do controlador.

Para executar uma cópia de segurança, terá de transferir as ferramentas do Azure Stack e, em seguida, execute o cmdlet do PowerShell **Start-AzSBackup** no seu nó de administração do Azure Stack. Para obter mais informações, consulte [cópia de segurança do Azure Stack](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Passos Seguintes

- Aprenda a executar uma cópia de segurança. Ver [cópia de segurança do Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Saiba como verificar se a cópia de segurança executou. Ver [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md).