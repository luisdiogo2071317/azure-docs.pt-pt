---
title: Ativar a cópia de segurança para a pilha do Azure no portal de administração da | Microsoft Docs
description: Ative o serviço de cópia de segurança de infraestrutura através do portal de administração para que a pilha do Azure pode ser restaurada se ocorrer uma falha.
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
ms.openlocfilehash: 0ef8247eba4605d3c8e5ef0992ce97bce989002e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Ativar a cópia de segurança para a pilha do Azure no portal de administração
Ative o serviço de cópia de segurança de infraestrutura através do portal de administração para que a pilha do Azure pode gerar cópias de segurança. Pode utilizar estas cópias de segurança para restaurar o seu ambiente na nuvem de recuperação em caso de a utilizar [uma falha catastrófica](.\azure-stack-backup-recover-data.md). O objetivo de recuperação na nuvem é garantir que os operadores e os utilizadores podem iniciar sessão no portal do após a conclusão da recuperação. Os utilizadores terão as respetivas subscrições restauradas incluindo permissões de acesso baseado em funções e as funções, planos originais, ofertas e a computação definida anteriormente, armazenamento e quotas de rede.

No entanto, o serviço de cópia de segurança de infraestrutura não cópia de segurança VMs de IaaS, configurações de rede e recursos de armazenamento, tais como contas de armazenamento, blobs, tabelas, e assim sucessivamente, para que os utilizadores iniciar sessão após a recuperação da nuvem concluída não verão qualquer um dos respetivos anteriormente existente recursos. Plataforma como serviço (PaaS) recursos e os dados são também não feitos pelo serviço. 

Os administradores e utilizadores são responsáveis pela cópia de segurança e restaurar recursos IaaS e PaaS separadamente a partir os processos de cópia de segurança da infraestrutura. Para obter informações sobre cópias de segurança a recursos IaaS e PaaS, consulte as seguintes ligações:

- [Máquinas Virtuais](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [Serviço de Aplicações](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


> [!Note]  
> Antes de ativar a cópia de segurança através da consola, terá de configurar o serviço de cópia de segurança. Pode configurar o serviço de cópia de segurança através do PowerShell. Para obter mais informações, consulte [ativar a cópia de segurança de pilha do Azure com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Ativar a cópia de segurança

1. Abra o portal de administração do Azure pilha em [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolha **configuração** no **cópia de segurança da infraestrutura** painel.

    ![Pilha do Azure - definições de controlador de cópia de segurança](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Utilize uma cadeia de convenção de Nomenclatura Universal (UNC) para o caminho para uma partilha de ficheiros alojado num dispositivo separado. Uma cadeia em UNC Especifica a localização dos recursos, tais como ficheiros partilhados ou dispositivos. Para o serviço, pode utilizar um endereço IP. Para garantir a disponibilidade dos dados de cópia de segurança depois de um desastre, o dispositivo deve estar num local separado.
    > [!Note]  
    > Se o seu ambiente suportar resolução do nome de rede de infraestrutura de pilha do Azure para o seu ambiente empresarial, pode utilizar um FQDN, em vez de IP.
4. Tipo de **Username** com o domínio e o nome de utilizador. Por exemplo, `Contoso\administrator`.
5. Tipo de **palavra-passe** para o utilizador.
5. Escreva a palavra-passe novamente a **Confirmar palavra-passe**.
6. Fornecer uma chave pré-partilhada no **chave de encriptação** caixa. Ficheiros de cópia de segurança estão encriptados com esta chave. Certifique-se armazenar esta chave numa localização segura. Depois de definir esta chave pela primeira vez ou rodar a chave no futuro, não é possível ver esta chave desta interface. Para obter mais instruções gerar uma chave pré-partilhada, siga os scripts em [ativar a cópia de segurança de pilha do Azure com o PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Selecione **OK** para guardar as definições de cópia de segurança do controlador.

Para executar uma cópia de segurança, tem de transferir as ferramentas de pilha do Azure e, em seguida, execute o cmdlet do PowerShell **início AzSBackup** no nó de administração a pilha do Azure. Para obter mais informações, consulte [cópia de segurança do Azure pilha](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Passos Seguintes

- Saiba como executar uma cópia de segurança. Consulte [cópia de segurança do Azure pilha](azure-stack-backup-back-up-azure-stack.md ).
- Saiba verificar que executou a cópia de segurança. Consulte [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md).
