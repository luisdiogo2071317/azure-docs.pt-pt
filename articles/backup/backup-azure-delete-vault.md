---
title: " Eliminar um cofre dos serviços de recuperação no Azure | Microsoft Docs "
description: "Este artigo explica como eliminar um cofre dos serviços de recuperação. O artigo inclui os passos de resolução de problemas quando tentar eliminar um cofre, mas não é possível."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/20/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: 4f4a92159b01b197984130c15195419e1b166fd3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="delete-a-recovery-services-vault"></a>Eliminar um cofre dos Serviços de Recuperação
Este artigo explica como eliminar um cofre dos serviços de recuperação no portal do Azure. Se tiver cofres de cópia de segurança, ter foi convertidos para cofres dos serviços de recuperação.   

Eliminar um cofre dos serviços de recuperação é um processo de um passo - *fornecido no Cofre não contém quaisquer recursos*. Antes de poder eliminar um cofre dos serviços de recuperação, tem de remover ou eliminar todos os recursos no cofre. Se tentar eliminar um cofre que contém os recursos, obter um erro, como a imagem seguinte:

![Erro de eliminação de cofre](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Até ter limpo os recursos do cofre, clicando em **repita** produz o mesmo erro. Se estiver bloqueada nesta mensagem de erro, clique em **Cancelar** e utilize os seguintes passos para eliminar os recursos no cofre.

## <a name="removing-items-from-a-vault-protecting-a-vm"></a>Remover itens de um cofre de proteger uma VM
Se já tiver o Cofre dos serviços de recuperação, abrir, avance para o segundo passo.

1. Abra o portal do Azure e a partir do Dashboard abrir o cofre que pretende eliminar.

   Se não tiver o Cofre de serviços de recuperação afixado ao Dashboard, no Hub menu, clique em **mais serviços** e, na lista de recursos, escreva **dos serviços de recuperação**. À medida que começa a escrever, a lista filtra com base na sua entrada. Clique em **cofres dos serviços de recuperação**.

   ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   É apresentada a lista de cofres dos serviços de recuperação. Na lista, selecione o cofre que pretende eliminar.

   ![Escolha o Cofre da lista](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. Na vista de cofre, observe o **Essentials** painel. Para eliminar um cofre, não pode ser qualquer itens protegidos. Se o **itens de cópia de segurança** ou **cópia de segurança de servidores de gestão** não mostrar zero, tem de remover os itens. Não é possível eliminar o Cofre se contém dados.

    ![Observe o painel Essentials para itens protegidos](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    VMs e os ficheiros/pastas são consideradas os itens de cópia de segurança e estão listadas no **itens de cópia de segurança** área do painel Essentials. Um servidor do DPM está listado no **cópia de segurança do servidor de gestão** área do painel Essentials. **Replicado itens** dizem respeito ao serviço Azure Site Recovery.
3. Para começar a remover os itens protegidos do cofre, encontrar os itens no cofre. No dashboard do cofre, clique em **definições**e, em seguida, clique em **itens de cópia de segurança** para abrir esse menu.

    ![Escolha o Cofre da lista](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    O **itens de cópia de segurança** menu tem listas separadas, consoante o tipo de Item: máquinas virtuais do Azure ou de pastas de ficheiro (consulte imagem). A lista de tipo de Item predefinido apresentada é Virtual Machines do Azure. Para ver a lista de itens de pastas de ficheiro no cofre, selecione **ficheiro pastas** no menu pendente.
4. Antes de poder eliminar um item a partir do Cofre de proteger uma VM, tem de parar a tarefa de cópia de segurança do item e eliminar os dados de ponto de recuperação. Para cada item no cofre, siga estes passos:

    a. No **itens de cópia de segurança** com o botão direito do item de menu e, no menu de contexto, selecione **cópia de segurança de paragem**.

    ![parar a tarefa de cópia de segurança](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Abre o menu de parar a cópia de segurança.

    b. No **parar a cópia de segurança** menu, do **escolher uma opção** menu, selecione **eliminar dados de cópia de segurança** > escreva o nome do item > e clique em **cópia de segurança de paragem**.

    Escreva o nome do item, certifique-se de que pretende eliminá-la. O **parar a cópia de segurança** botão ativa depois de verificar o item. Se não vir a caixa de diálogo, escreva o nome do item de cópia de segurança, que escolheu a **reter dados de cópia de segurança** opção.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Opcionalmente, pode indicar um motivo por que motivo está a eliminar os dados e adicionar os seus comentários. Depois de clicar em **parar a cópia de segurança**, permitir que a tarefa de eliminação seja concluída antes de tentar eliminar o cofre. Para verificar que a tarefa foi concluída, verifique as mensagens do Azure ![eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/messages.png). <br/>
    Assim que a tarefa estiver concluída, o serviço envia uma mensagem: o processo de cópia de segurança foi parado e os dados de cópia de segurança foi eliminados.

    c. Depois de eliminar um item na lista, no **itens de cópia de segurança** menu, clique em **atualizar** para ver os itens restantes no cofre.

      ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/empty-items-list.png)

      Quando existem não existem itens na lista, desloque-se para o **Essentials** painel no menu do Cofre de serviços de recuperação. Não deve existir qualquer **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens** listados. Se ainda forem apresentados itens no cofre, volte ao passo três e escolha uma lista de tipo de item diferentes.  
5. Quando não existe nenhum mais itens na barra de ferramentas do cofre, clique em **eliminar**.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-vault.png)
6. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>E se o processo de cópia de segurança foi parado mas manter os dados?
Se parar o processo de cópia de segurança, mas acidentalmente *retidos* os dados, tem de eliminar os dados de cópia de segurança antes de poder eliminar o cofre. Para eliminar os dados de cópia de segurança:

1. No **itens de cópia de segurança** menu, clique com o botão direito do item e, no menu de contexto, clique em **eliminar dados de cópia de segurança**.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    O **eliminar dados de cópia de segurança** é aberto o menu.
2. No **eliminar dados de cópia de segurança** menu, escreva o nome do item e clique em **eliminar**.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Depois de ter eliminado os dados, volte ao passo 4c e continuar o processo.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Eliminar um cofre utilizado para proteger um servidor do DPM
Antes de poder eliminar um cofre utilizado para proteger um servidor do DPM, terá de limpar pontos de recuperação que foram criados e, em seguida, anular o registo do servidor do cofre.

Para eliminar os dados associados um grupo de proteção:

1. Na consola do administrador do DPM, clique em **proteção** > selecione um grupo de proteção > selecione o membro do grupo de proteção > e, no Friso de ferramentas, clique em **remover**.

  Selecione o membro do grupo de proteção para ativar o **remover** botão no Friso de ferramentas. No exemplo, é o membro **dummyvm9**. Para selecionar vários membros do grupo de proteção, mantenha premida a tecla Ctrl ao clicar em membros.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    O **parar proteção** abre a caixa de diálogo.
2. No **parar proteção** caixa de diálogo, selecione **eliminar dados protegidos**e clique em **parar proteção**.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Para eliminar um cofre, tem de desmarcar ou eliminar o Cofre dos dados protegidos. Se existirem vários pontos de recuperação e os dados no grupo de proteção, pode demorar alguns minutos para eliminar os dados. O **parar proteção** caixa de diálogo mostra quando a tarefa foi concluída.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Continue este processo para todos os membros de todos os grupos de proteção.

    Remova grupos de proteção de dados e todos os protegidos.
4. Depois de eliminar todos os membros do grupo de proteção, mude para o portal do Azure. Abra o dashboard do cofre e certifique-se de que existem não **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens**. Na barra de ferramentas cofre, clique em **eliminar**.

    ![Eliminar dados de cópia de segurança](./media/backup-azure-delete-vault/delete-vault.png)

    Se existirem servidores de gestão de cópia de segurança registados no cofre, não é possível eliminar o cofre, mesmo se houver sem dados no cofre. Se eliminar os servidores de gestão de cópia de segurança associados ao cofre, mas existem servidores indicados a **Essentials** painel, consulte [localizar os servidores de gestão de cópia de segurança registados no cofre](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault).
5. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Eliminar um cofre utilizado para proteger um servidor de produção
Antes de poder eliminar um cofre utilizado para proteger um servidor de produção, tem de eliminar ou anular o registo do servidor do cofre.

Ao eliminar o servidor de produção associado ao Cofre:

1. No portal do Azure, abra o dashboard do cofre e clique em **definições** > **infraestrutura de cópia de segurança** > **servidores de produção**.

    ![Abra o menu de servidores de produção](./media/backup-azure-delete-vault/delete-production-server.png)

    O **servidores de produção** menu abre e apresenta uma lista de todos os servidores de produção no cofre.

    ![lista de servidores de produção](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. No **servidores de produção** menu, clique com o botão direito no servidor e clique em **eliminar**.

    ![eliminar o servidor de produção ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    O **eliminar** é aberto o menu.

    ![eliminar o servidor de produção ](./media/backup-azure-delete-vault/delete-blade.png)
3. No **eliminar** menu, confirme o nome do servidor e clique em **eliminar**. Corretamente tem o nome do servidor, para ativar o **eliminar** botão.

    Depois de eliminar o cofre, receberá uma mensagem a indicar que o Cofre foi eliminado. Depois de eliminar todos os servidores no cofre, desloque-se novamente para o painel Essentials no dashboard do cofre.
4. No dashboard do cofre, certifique-se de que existem não **itens de cópia de segurança**, **cópia de segurança de servidores de gestão**, ou **replicado itens**. Na barra de ferramentas cofre, clique em **eliminar**.
5. Certifique-se de que pretende eliminar o cofre, clique em **Sim**.

    O Cofre é eliminado e devolve o portal e o **novo** menu de serviço.

## <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Localizar os servidores de gestão de cópia de segurança registados no Cofre
Se tiver vários servidores registados para um cofre, pode ser difícil de recordá-los. Para ver os servidores registados no cofre e eliminá-los:

1. Abra o dashboard do cofre.
2. No **Essentials** painel, clique em **definições** para abrir esse menu.

    ![Abra o menu de definições](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. No **definições** menu, clique em **infraestrutura de cópia de segurança**.
4. No **infraestrutura de cópia de segurança** menu, clique em **servidores de gestão de cópia de segurança**. Abre o menu de servidores de gestão de cópia de segurança.

    ![lista de servidores de gestão de cópia de segurança](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Para eliminar um servidor na lista, clique no nome do servidor e, em seguida, clique em **eliminar**.
    O **eliminar** é aberto o menu.
6. No **eliminar** menu, forneça o nome do servidor. Se se tratar de um nome por extenso, pode copiar e colá-lo da lista de servidores de gestão de cópia de segurança. Em seguida, clique em **eliminar**.  
