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
ms.date: 08/16/2018
ms.author: jeffgilb
ms.openlocfilehash: 6231ee760902618afedf64443690be0b02c4d0eb
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42061523"
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Ativar cópia de segurança para o Azure Stack a partir do portal de administração
Ative o serviço de cópia de segurança de infraestrutura através do portal de administração para que o Azure Stack pode gerar as cópias de segurança. Pode utilizar estas cópias de segurança para restaurar o seu ambiente na cloud de recuperação na eventualidade de a utilizar [uma falha catastrófica](.\azure-stack-backup-recover-data.md). O objetivo de recuperação na cloud é garantir que seus operadores e os utilizadores podem iniciar sessão no portal do após a conclusão da recuperação. Os utilizadores terão suas assinaturas restauradas, incluindo permissões de acesso baseado em funções e funções, originais planos, ofertas e a computação definidos anteriormente, armazenamento e quotas de rede.

No entanto, o serviço de cópia de segurança de infraestrutura não cópia de segurança de VMs de IaaS, configurações de rede e recursos de armazenamento, como contas de armazenamento, blobs, tabelas, e assim por diante, para que os utilizadores iniciar sessão após a recuperação de nuvem é concluído não verão qualquer um dos anteriormente existente recursos. Plataforma como serviço (PaaS) recursos e os dados são também não backup pelo serviço. 

Os administradores e utilizadores são responsáveis por fazer backup e restaurar os recursos de IaaS e PaaS separadamente dos processos de cópia de segurança da infraestrutura. Para obter informações sobre cópias de segurança recursos de IaaS e PaaS, consulte as seguintes ligações:

- [Máquinas Virtuais](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-protect)
- [Serviço de Aplicações](https://docs.microsoft.com/azure/app-service/web-sites-backup)
- [SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)


## <a name="enable-or-reconfigure-backup"></a>Ativar ou reconfigurar a cópia de segurança

1. Abra o [portal de administração do Azure Stack](azure-stack-manage-portals.md).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolher **Configuration** no **cópia de segurança da infraestrutura** painel.
3. Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Utilize uma cadeia de caracteres de convenção de Nomenclatura Universal (UNC) para o caminho para uma partilha de ficheiros hospedado num dispositivo separado. Uma cadeia de caracteres UNC Especifica a localização de recursos, tais como ficheiros partilhados ou dispositivos. Para o serviço, pode utilizar um endereço IP. Para garantir a disponibilidade dos dados de cópia de segurança após um desastre, o dispositivo deve estar num local separado.

    > [!Note]  
    > Se o ambiente oferecer suporte a resolução do nome de rede de infraestrutura do Azure Stack para seu ambiente empresarial, pode utilizar um FQDN, em vez do IP.
    
4. Tipo de **nome de utilizador** usando o domínio e nome de utilizador com acesso suficiente para ler e escrever ficheiros. Por exemplo, `Contoso\backupshareuser`.
5. Tipo de **palavra-passe** para o utilizador.
6. Escreva a palavra-passe novamente ao **Confirmar palavra-passe**.
7. O **frequência em horas** determina com que frequência as cópias de segurança são criadas. O valor predefinido é 12. O Scheduler suporta um máximo de 12 e um mínimo de 4. 
8. O **período de retenção em dias** determina o número de dias de cópias de segurança é mantido na localização externa. O valor predefinido é de 7. O Scheduler suporta um máximo de 14 e um mínimo de 2. As cópias de segurança mais antigas do que o período de retenção são automaticamente eliminadas da localização externa.

    > [!Note]  
    > Se pretende arquivar as cópias de segurança mais antigas do que o período de retenção, lembre-se de que os ficheiros de cópia de segurança antes do agendador elimina as cópias de segurança. Se reduzir o período de retenção de cópia de segurança (por exemplo, a partir de 7 dias para 5 dias), o agendador eliminará todas as cópias de segurança mais antigas do que o novo período de retenção. Certifique-se de que tiver problemas com as cópias de segurança introdução eliminadas antes de atualizar este valor. 

9. Fornecer uma chave pré-partilhada no **chave de encriptação** caixa. Ficheiros de cópia de segurança são encriptados com esta chave. Certifique-se armazenar esta chave numa localização segura. Depois de definir esta chave pela primeira vez ou rotação da chave no futuro, não pode ver a chave a partir dessa interface. Para criar a chave, execute os seguintes comandos do Azure Stack do PowerShell:
    ```powershell
    New-AzsEncryptionKeyBase64
    ```
10. Selecione **OK** para guardar as definições de cópia de segurança do controlador.

    ![O Azure Stack - definições do controlador de cópia de segurança](media\azure-stack-backup\backup-controller-settings.png)

## <a name="start-backup"></a>Iniciar cópia de segurança
Para iniciar uma cópia de segurança, clique em **agora a cópia de segurança** para iniciar uma cópia de segurança a pedido. Uma cópia de segurança a pedido não irá modificar a hora para o próximo backup agendado. Depois da tarefa estiver concluída, pode confirmar as definições no **Essentials**:

![O Azure Stack - cópia de segurança a pedido](media\azure-stack-backup\scheduled-backup.png).

Também pode executar o cmdlet do PowerShell **Start-AzsBackup** no seu computador de administração do Azure Stack. Para obter mais informações, consulte [cópia de segurança do Azure Stack](azure-stack-backup-back-up-azure-stack.md).

## <a name="enable-or-disable-automatic-backups"></a>Ativar ou desativar as cópias de segurança automáticas
As cópias de segurança agendadas automaticamente ao ativar a cópia de segurança. Pode verificar a próxima hora de cópia de segurança agendada na **Essentials**. 

![O Azure Stack - cópia de segurança a pedido](media\azure-stack-backup\on-demand-backup.png)

Se tiver de desativar futuras cópias de segurança agendadas, clique em **desativar cópias de segurança automáticas**. Desativar as cópias de segurança automática irão manter as definições de cópia de segurança configuradas e irão reter a agenda de cópia de segurança. Esta ação simplesmente diz ao scheduler para ignorar futuras cópias de segurança. 

![O Azure Stack - desativar agendado cópias de segurança](media\azure-stack-backup\disable-auto-backup.png)

Confirme que futuras cópias de segurança agendadas foram desativadas na **Essentials**:

![O Azure Stack - confirmar as cópias de segurança foram desativadas](media\azure-stack-backup\confirm-disable.png)

Clique em **habilitar cópias de segurança automáticas** para informar o scheduler para iniciar cópias de segurança futuras no horário programado. 

![O Azure Stack - ativar agendado cópias de segurança](media\azure-stack-backup\enable-auto-backup.png)


> [!Note]  
> Se tiver configurado a cópia de segurança da infraestrutura antes de atualizar para 1807, cópias de segurança automáticas serão desativadas. Desta forma as cópias de segurança iniciadas pelo Azure Stack não entram em conflito com cópias de segurança iniciadas por uma motor de agendamento de tarefas externas. Depois de desativar qualquer programador de tarefas externa, clique em **habilitar cópias de segurança automáticas**.


## <a name="next-steps"></a>Passos Seguintes

- Aprenda a executar uma cópia de segurança. Ver [cópia de segurança do Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Saiba como verificar se a cópia de segurança executou. Ver [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md).
