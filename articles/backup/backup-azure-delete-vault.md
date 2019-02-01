---
title: Eliminar um cofre dos serviços de recuperação do Azure "
description: Este artigo explica como eliminar um cofre dos serviços de recuperação. O artigo inclui passos de resolução de problemas ao tentar eliminar um cofre, mas não é possível.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: d514074e56ff37cc7af6a97ea86aa9e02e3763e0
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492288"
---
# <a name="delete-a-recovery-services-vault"></a>Eliminar um cofre dos Serviços de Recuperação

Este artigo descreve como remover todos os itens de um cofre dos serviços de recuperação e, em seguida, eliminá-lo. Não é possível eliminar um cofre dos serviços de recuperação se ele está registado para um servidor e contém dados de cópia de segurança. Se tentar eliminar um cofre, mas não é possível, o Cofre ainda está configurado para receber dados de cópia de segurança.

Para saber como eliminar um cofre, consulte a secção [eliminar um cofre a partir do portal do Azure](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Se não pretender reter dados no cofre dos serviços de recuperação e pretende eliminar o cofre, consulte a secção [a eliminação do cofre por força](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Se não souber ao certo o que é no cofre e terá de certificar-se de que pode eliminar o cofre, consulte a secção [remover dependências de cofre e eliminar cofre](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Eliminar um cofre a partir do portal do Azure

Se já tiver o Cofre de serviços de recuperação aberto, avance para o segundo passo.

1. Abra o portal do Azure e a partir do Dashboard, abra o cofre que pretende eliminar.

   Se não tiver o Cofre de serviços de recuperação afixado ao Dashboard, no Hub menu, clique em **todos os serviços** e, na lista de recursos, escreva **dos serviços de recuperação**. À medida que começa a escrever, a lista filtra com base na sua entrada. Para ver a lista de cofres na sua subscrição, clique em **cofres dos serviços de recuperação**.

   ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   É apresentada a lista de cofres dos serviços de recuperação. 

   ![Escolha o Cofre na lista](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

1. Na lista, selecione o cofre que pretende eliminar. Quando selecionar o cofre, é aberto o dashboard do cofre.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Para eliminar um cofre, no dashboard do cofre, clique em **eliminar**. Será solicitado a certifique-se de que pretende eliminar o cofre.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Se o **erro ao eliminar o Cofre dos** aparece, pode optar por remover as dependências do cofre, ou pode utilizar o PowerShell para eliminar o Cofre por força. As secções seguintes explicam como realizar estas tarefas.

    ![Erro de eliminação do cofre](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Eliminar o Cofre de serviços de recuperação por força

Pode utilizar o PowerShell para eliminar um cofre de serviços de recuperação por força. Por meio da força o Cofre dos serviços de recuperação e associados todos os dados de cópia de segurança, é eliminado permanentemente. 

> [!Warning]
> Quando utilizar o PowerShell para eliminar um cofre dos serviços de recuperação, certifique-se de que pretende eliminar permanentemente todos os dados de cópia de segurança no cofre.
>

Para eliminar um cofre dos serviços de recuperação:

1. Inicie sessão na sua conta do Azure.

   Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

   ```powershell
    Connect-AzureRmAccount
   ```
   Da primeira vez que utilizar o Azure Backup, tem de registar o fornecedor do Serviço de Recuperação do Azure na sua subscrição com [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider).

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

1. Abra uma janela do PowerShell com privilégios de administrador.

1. Utilize `Set-ExecutionPolicy Unrestricted` para remover quaisquer restrições.

1. Execute o seguinte comando para transferir o pacote de cliente do Azure Resource Manager de chocolately.org.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

1. Utilize o seguinte comando para instalar o cliente de API do Azure Resource Manager.

   `choco.exe install armclient`

1. No portal do Azure, recolha o ID de subscrição e o nome do grupo de recursos associados para o Cofre de serviços de recuperação que pretende eliminar.

1. No PowerShell, execute o seguinte comando com o seu ID de subscrição, nome do grupo de recursos e o nome do cofre dos serviços de recuperação. Ao executar o comando, elimina o Cofre e todas as dependências.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
   O Cofre tem de estar vazio antes de poder eliminá-lo. Caso contrário, receberá um erro de citação de "Cofre não pode ser eliminado porque existem recursos existentes dentro deste cofre". O comando seguinte demonstra como remover um contentor de dentro de um cofre:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```
   
1. Inicie sessão na sua subscrição no portal do Azure e certifique-se de que o Cofre é eliminado.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Remover dependências do cofre e eliminar Cofre

Para remover manualmente as dependências de cofre, elimine a configuração entre cada item ou o servidor e o Cofre dos serviços de recuperação. À medida que avança o procedimento seguinte, utilize o **itens de cópia de segurança** menu (veja a imagem) para:

* Cópias de segurança de armazenamento (ficheiros do Azure) do Azure
* SQL Server em cópias de segurança de VM do Azure
* Cópias de segurança de máquinas virtuais do Azure
* Cópias de segurança de agente de serviços de recuperação do Microsoft Azure

Utilize o **infraestrutura de cópia de segurança** menu (veja a imagem) para:

* Cópias de segurança de servidor de cópia de segurança do Azure
* Cópias de segurança do System Center DPM

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. No menu do dashboard do cofre, desloque para baixo para a secção de itens protegidos e clique em **itens de cópia de segurança**. Neste menu, pode parar e eliminar servidores de ficheiros do Azure, servidores SQL na VM do Azure e máquinas virtuais do Azure. Neste exemplo, podemos remover dados de cópia de segurança de um servidor de ficheiros do Azure.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/selected-backup-items.png)

1. Selecione um tipo de cópia de segurança para ver todos os itens desse tipo.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

1. Para todos os itens na lista, com o botão direito do item e, no menu de contexto, selecione **parar cópia de segurança**.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-item.png) 

    É aberto o menu parar cópia de segurança.

1. Sobre o **parar cópia de segurança** menu, da **escolha uma opção** menu, selecione **eliminar dados de cópia de segurança**, escreva o nome do item e clique em **parar cópia de segurança**.

    Escreva o nome do item, a certifique-se de que pretende eliminá-lo. O **parar cópia de segurança** botão ativa depois de verificar o item. Se mantiver os dados, não poderá eliminar o cofre.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Se pretender, forneça um motivo por que motivo está a eliminar os dados e adicione comentários. Para verificar a tarefa foi concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Assim que a tarefa é concluída, o serviço envia uma mensagem: *o processo de cópia de segurança foi parado e os dados de cópia de segurança foram eliminados*.

1. Depois de eliminar um item na lista, sobre o **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando não há nenhum item na lista, desloque-se para o **Essentials** painel no menu do cofre dos serviços de recuperação. Não deve haver algum **itens de cópia de segurança**, **servidores de gestão de cópia de segurança**, ou **itens replicados** listados. Se ainda forem apresentados itens no cofre, regresse ao passo três e escolha uma lista de tipo de item de diferentes.  

1. Quando não há mais itens da barra de ferramentas do cofre, clique em **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e o portal devolve para o **New** menu de serviços.

## <a name="removing-azure-backup-server-or-dpm"></a>Remover servidor de cópia de segurança do Azure ou o DPM

1. No menu do dashboard do cofre, desloque para baixo para a secção Gerir e clique em **infraestrutura de cópia de segurança**. 

1. No submenu, clique em **servidores de gestão de cópia de segurança** para ver o servidor de servidores de cópia de segurança do Azure e o System Center DPM. Pode parar e eliminar servidores de ficheiros do Azure, servidores SQL na VM do Azure e máquinas virtuais do Azure. 

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

1. Com o botão direito do item que pretende eliminar e, de submenu, selecione **eliminar**.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    É aberto o menu parar cópia de segurança.

1. Sobre o **parar cópia de segurança** menu, da **escolha uma opção** menu, selecione **eliminar dados de cópia de segurança**, escreva o nome do item e clique em **parar cópia de segurança**.

    Para verificar-se de que pretende eliminar, digite seu nome. O **parar cópia de segurança** botão ativa depois de verificar o item. Se mantiver os dados, não é possível eliminar o cofre.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Opcionalmente, pode fornecer um motivo por que motivo está a eliminar os dados e adicione comentários. Para verificar que a tarefa for concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Quando a tarefa estiver concluída, o serviço envia uma mensagem: o processo de cópia de segurança foi parado e os dados de cópia de segurança foram eliminados.

1. Depois de eliminar um item na lista, sobre o **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens restantes no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando não há nenhum item na lista, desloque-se para o **Essentials** painel no menu do cofre dos serviços de recuperação. Não deve haver algum **itens de cópia de segurança**, **servidores de gestão de cópia de segurança**, ou **itens replicados** listados. Se ainda forem apresentados itens no cofre, regresse ao passo três e escolha uma lista de tipo de item de diferentes.  
1. Quando existem não existem mais itens no cofre, no dashboard do cofre, clique **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e o portal devolve para o **New** menu de serviços.


## <a name="removing-azure-backup-agent-recovery-points"></a>Remoção de pontos de recuperação de agente de cópia de segurança do Azure

1. No menu do dashboard do cofre, desloque para baixo para a secção Gerir e clique em **infraestrutura de cópia de segurança**.

1. No submenu, clique em **servidores protegidos** para ver a lista de protegidos tipos de servidor, incluindo o agente de cópia de segurança do Azure.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/identify-protected-servers.png)

1. Na **servidores protegidos** , clique em agente de cópia de segurança do Azure.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    A lista de servidores protegidos com o agente de cópia de segurança do Azure, é aberto.

    ![Selecione o servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

1. Na lista de servidores, clique em um para abrir respetivo menu.

    ![ver o dashboard do servidor selecionado](./media/backup-azure-delete-vault/selected-protected-server.png)

1. No menu do dashboard do servidor selecionado, clique em **eliminar**.

    ![eliminar o servidor selecionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

1. Sobre o **elimine** escreva o nome do item de menu e clique em **eliminar**.

    Escreva o nome do item, a certifique-se de que pretende eliminá-lo. O **eliminar** botão ativa depois de verificar o item.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    Opcionalmente, pode fornecer um motivo por que motivo está a eliminar os dados e adicione comentários. Para verificar que a tarefa for concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Quando a tarefa estiver concluída, o serviço envia uma mensagem: o processo de cópia de segurança foi parado e os dados de cópia de segurança foram eliminados.

1. Depois de eliminar um item na lista, sobre o **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens restantes no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando não há nenhum item na lista, desloque-se para o **Essentials** painel no menu do cofre dos serviços de recuperação. Não deve haver algum **itens de cópia de segurança**, **servidores de gestão de cópia de segurança**, ou **itens replicados** listados. Se ainda forem apresentados itens no cofre, regresse ao passo três e escolha uma lista de tipo de item de diferentes.  
1. Quando existem não existem mais itens no cofre, no dashboard do cofre, clique **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e o portal devolve para o **New** menu de serviços.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>E se parar o processo de cópia de segurança mas manter os dados?

Se parar o processo de cópia de segurança, mas acidentalmente *reter* os dados, tem de eliminar os dados de cópia de segurança antes de poder eliminar o cofre. Para eliminar os dados de cópia de segurança:

1. Sobre o **itens de cópia de segurança** menu, clique com o botão direito do item e, no menu de contexto, clique em **eliminar dados de cópia de segurança**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    O **eliminar dados de cópia de segurança** é aberto o menu.
1. Sobre o **eliminar dados de cópia de segurança** escreva o nome do item de menu e clique em **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Depois de ter eliminado os dados, regresse ao passo 4c e continuar com o processo.
