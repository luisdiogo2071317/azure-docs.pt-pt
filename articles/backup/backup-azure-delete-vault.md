---
title: Eliminar um cofre dos serviços de recuperação no Azure '
description: Este artigo explica como eliminar um cofre dos serviços de recuperação. O artigo inclui os passos de resolução de problemas quando tentar eliminar um cofre, mas não é possível.
services: service-name
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 6/21/2018
ms.author: markgal
ms.openlocfilehash: d8169eba6790e49a85d69434663faabe7430942e
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937605"
---
# <a name="delete-a-recovery-services-vault"></a>Eliminar um cofre dos Serviços de Recuperação

Este artigo explica como remover todos os itens de um cofre dos serviços de recuperação e elimine-o. Não é possível eliminar um cofre dos serviços de recuperação se este se encontra registada junto de um servidor e contém dados de cópia de segurança. Se tentar eliminar um cofre, mas não é possível, o Cofre ainda está configurado para receber dados de cópia de segurança.

Para saber como eliminar um cofre, consulte a secção [eliminar um cofre a partir do portal do Azure](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal). Se não pretender manter os dados no cofre dos serviços de recuperação e pretende eliminar o cofre, consulte a secção [eliminar o Cofre de maneira forçada](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force). Se não tem a certeza Novidades no cofre e tem de certificar-se de que pode eliminar o cofre, consulte a secção [remover dependências de cofre e eliminar cofre](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault).

## <a name="delete-a-vault-from-azure-portal"></a>Eliminar um cofre a partir do portal do Azure

Se já tiver o Cofre dos serviços de recuperação, abrir, avance para o segundo passo.

1. Abra o portal do Azure e a partir do Dashboard abrir o cofre que pretende eliminar.

   Se não tiver o Cofre de serviços de recuperação afixado ao Dashboard, no Hub menu, clique em **todos os serviços** e, na lista de recursos, escreva **dos serviços de recuperação**. À medida que começa a escrever, a lista filtra com base na sua entrada. Para ver a lista de cofres na sua subscrição, clique em **cofres dos serviços de recuperação**.

   ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   É apresentada a lista de cofres dos serviços de recuperação. 

   ![Escolha o Cofre da lista](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

2. Na lista, selecione o cofre que pretende eliminar. Quando seleciona o cofre, abre o dashboard do cofre.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

3. Para eliminar um cofre, no dashboard do cofre, clique em **eliminar**. Irá ser-lhe pedido para confirmar que pretende eliminar o cofre.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Se o **erro de eliminação de cofre** for apresentada, pode optar por remover as dependências do cofre, ou pode utilizar o PowerShell para eliminar o Cofre de maneira forçada. As secções seguintes explicam como realizar estas tarefas.

    ![Erro de eliminação de cofre](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Elimine o Cofre de serviços de recuperação de maneira forçada

Pode utilizar o PowerShell para eliminar um cofre dos serviços de recuperação por force. Meios de forçar o Cofre dos serviços de recuperação e associados todos os dados de cópia de segurança, é eliminado permanentemente. 

> [!Warning]
> Quando utilizar o PowerShell para eliminar um cofre dos serviços de recuperação, ter a certeza de que pretende eliminar permanentemente todos os dados de cópia de segurança no cofre.
>

Para eliminar um cofre dos serviços de recuperação:

1. Inicie sessão sua conta do Azure.

   Inicie sessão na sua subscrição do Azure com o `Connect-AzureRmAccount` de comandos e siga no ecrã as direções.

   ```powershell
    Connect-AzureRmAccount
   ```
   Da primeira vez que utilizar o Azure Backup, tem de registar o fornecedor do Serviço de Recuperação do Azure na sua subscrição com [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider).

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

2. Abra uma janela do PowerShell com privilégios de administrador.

3. Utilize `Set-ExecutionPolicy Unrestricted` para remover quaisquer restrições.

4. Execute o seguinte comando para transferir o pacote de cliente do Azure Resource Manager a partir de chocolately.org.

    `iex ((New-Object System.Net.WebClient) DownloadString('https://chocolatey.org/install.ps1))`

5. Utilize o seguinte comando para instalar o cliente de API do Azure Resource Manager.

   `choco.exe install armclient`

6. No portal do Azure, recolha o ID de subscrição e o nome do grupo de recursos associados para o Cofre de serviços de recuperação que pretende eliminar.

7. No PowerShell, execute o seguinte comando utilizando o seu ID de subscrição, o nome do grupo de recursos e o nome do cofre dos serviços de recuperação. Quando executar o comando, elimina o Cofre e todas as dependências.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
8. Inicie sessão na sua subscrição no portal do Azure e certifique-se de que o Cofre é eliminado.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Remover as dependências do cofre e eliminar o Cofre

Para remover manualmente as dependências do cofre, elimine a configuração entre cada item ou o servidor e o Cofre dos serviços de recuperação. À medida que avança o procedimento seguinte, utilize o **itens de cópia de segurança** menu (consulte imagem) para:

* Cópias de segurança de armazenamento (ficheiros do Azure) do Azure
* SQL Server em cópias de segurança de VM do Azure
* Cópias de segurança de máquinas virtuais do Azure
* Cópias de segurança do Microsoft Azure Recovery Services agent

Utilize o **infraestrutura de cópia de segurança** menu (consulte imagem) para:

* Cópias de segurança de servidor do Backup do Azure
* Cópias de segurança do System Center DPM

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. No menu do dashboard do cofre, desloque para baixo para a secção de itens protegidos e clique em **itens de cópia de segurança**. Este menu, pode parar e eliminar servidores de ficheiros do Azure, servidores de SQL Server numa VM do Azure e máquinas virtuais do Azure. Neste exemplo, iremos remover dados de cópia de segurança de um servidor de ficheiros do Azure.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/selected-backup-items.png)

2. Selecione um tipo de cópia de segurança para ver todos os itens desse tipo.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. Para todos os itens na lista, o item com o botão direito e, no menu de contexto, selecione **cópia de segurança de paragem**.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-item.png) 

    Abre o menu de parar a cópia de segurança.

4. No **parar a cópia de segurança** menu, do **escolher uma opção** menu, selecione **eliminar dados de cópia de segurança**, escreva o nome do item e clique em **cópia de segurança de paragem**.

    Escreva o nome do item, certifique-se de que pretende eliminá-la. O **parar a cópia de segurança** botão ativa depois de verificar o item. Se o utilizador retém os dados, não poderá eliminar o cofre.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Se quiser, indicar um motivo por que motivo está a eliminar os dados e adicionar os seus comentários. Para verificar a tarefa foi concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Uma vez concluída a tarefa, o serviço envia uma mensagem: *o processo de cópia de segurança foi parado e os dados de cópia de segurança foram eliminados*.

5. Depois de eliminar um item na lista, no **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando existem não existem itens na lista, desloque-se para o **Essentials** painel no menu do Cofre de serviços de recuperação. Não deve existir qualquer **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens** listados. Se ainda forem apresentados itens no cofre, volte ao passo três e escolha uma lista de tipo de item diferentes.  

6. Quando não existe nenhum mais itens na barra de ferramentas do cofre, clique em **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.

## <a name="removing-azure-backup-server-or-dpm"></a>Remover o servidor de cópia de segurança do Azure ou DPM

1. No menu do dashboard do cofre, desloque para baixo para a secção de gerir e clique em **infraestrutura de cópia de segurança**. 

2. No submenu, clique em **servidores de gestão de cópia de segurança** para ver o servidor de servidores de cópia de segurança do Azure e o System Center DPM. Pode parar e eliminar servidores de ficheiros do Azure, servidores de SQL Server numa VM do Azure e máquinas virtuais do Azure. 

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Clique com o botão direito do item que pretende eliminar e, no menu secundárias, selecione **eliminar**.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Abre o menu de parar a cópia de segurança.

4. No **parar a cópia de segurança** menu, do **escolher uma opção** menu, selecione **eliminar dados de cópia de segurança**, escreva o nome do item e clique em **cópia de segurança de paragem**.

    Para verificar-se de que pretende eliminar, escreva o respetivo nome. O **parar a cópia de segurança** botão ativa depois de verificar o item. Se o utilizador retém os dados, não é possível eliminar o cofre.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Opcionalmente, pode indicar um motivo por que motivo está a eliminar os dados e adicionar os seus comentários. Para verificar que a tarefa foi concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Assim que a tarefa estiver concluída, o serviço envia uma mensagem: o processo de cópia de segurança foi parado e os dados de cópia de segurança foi eliminados.

5. Depois de eliminar um item na lista, no **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens restantes no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando existem não existem itens na lista, desloque-se para o **Essentials** painel no menu do Cofre de serviços de recuperação. Não deve existir qualquer **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens** listados. Se ainda forem apresentados itens no cofre, volte ao passo três e escolha uma lista de tipo de item diferentes.  
6. Quando existem não existem mais itens no cofre, no dashboard do cofre clique **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

7. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.


## <a name="removing-azure-backup-agent-recovery-points"></a>Remoção de pontos de recuperação de agente do Backup do Azure

1. No menu do dashboard do cofre, desloque para baixo para a secção de gerir e clique em **infraestrutura de cópia de segurança**.

2. No menu secundárias, clique em **servidores protegidos** para ver a lista de protegidos tipos de servidor, incluindo o agente do Backup do Azure.

    ![Selecione o cofre para abrir o dashboard](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. No **servidores protegidos** lista, clique no agente de cópia de segurança do Azure.

    ![Selecione o tipo de cópia de segurança](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    A lista de servidores protegidos com o agente do Backup do Azure, é aberto.

    ![Selecione o servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

4. Na lista de servidores, clique num para abrir o menu.

    ![ver o dashboard do servidor selecionado](./media/backup-azure-delete-vault/selected-protected-server.png)

5. No menu do dashboard do servidor selecionado, clique em **eliminar**.

    ![eliminar o servidor selecionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. No **eliminar** menu, escreva o nome do item e clique em **eliminar**.

    Escreva o nome do item, certifique-se de que pretende eliminá-la. O **eliminar** botão ativa depois de verificar o item.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    Opcionalmente, pode indicar um motivo por que motivo está a eliminar os dados e adicionar os seus comentários. Para verificar que a tarefa foi concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Assim que a tarefa estiver concluída, o serviço envia uma mensagem: o processo de cópia de segurança foi parado e os dados de cópia de segurança foi eliminados.

7. Depois de eliminar um item na lista, no **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens restantes no cofre.

      ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando existem não existem itens na lista, desloque-se para o **Essentials** painel no menu do Cofre de serviços de recuperação. Não deve existir qualquer **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens** listados. Se ainda forem apresentados itens no cofre, volte ao passo três e escolha uma lista de tipo de item diferentes.  
8. Quando existem não existem mais itens no cofre, no dashboard do cofre clique **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

9. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>E se parar o processo de cópia de segurança mas manter os dados?

Se parar o processo de cópia de segurança, mas acidentalmente *manter* os dados, tem de eliminar os dados de cópia de segurança antes de poder eliminar o cofre. Para eliminar os dados de cópia de segurança:

1. No **itens de cópia de segurança** menu, clique com o botão direito do item e, no menu de contexto, clique em **eliminar dados de cópia de segurança**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    O **eliminar dados de cópia de segurança** é aberto o menu.
2. No **eliminar dados de cópia de segurança** menu, escreva o nome do item e clique em **eliminar**.

    ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Depois de ter eliminado os dados, volte ao passo 4c e continuar o processo.
