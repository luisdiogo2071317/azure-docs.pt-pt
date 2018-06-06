---
title: incluir ficheiro
description: incluir ficheiro
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 5590da80a1c217e7902e8e010688e40f5624898c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34664960"
---
## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 
Um cofre dos serviços de recuperação é uma entidade que armazena as cópias de segurança e os pontos de recuperação que foram criados ao longo do tempo. O Cofre de serviços de recuperação também contém as políticas de cópia de segurança que estão associadas a máquinas virtuais protegidas.

Para criar um cofre dos Serviços de Recuperação:

1. Inicie sessão na sua subscrição no [portal do Azure](https://portal.azure.com/).
2. No menu da esquerda, selecione **todos os serviços**.

    ![Escolha a opção de todos os serviços no menu principal](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. Na caixa de todos os serviços diálogo, escreva *dos serviços de recuperação*. À medida que começa a escrever, a sua entrada filtra a lista de recursos. Depois de o vir, selecione **cofres dos serviços de recuperação**.

    ![Tipo dos serviços de recuperação na caixa de diálogo todos os serviços](./media/backup-create-rs-vault/all-services.png) <br/>

    É apresentada a lista de cofres dos serviços de recuperação na subscrição.
4. No **cofres dos serviços de recuperação** menu, selecione **adicionar**.

    ![Passo 2 da Criação do Cofre dos Serviços de Recuperação](./media/backup-create-rs-vault/add-button-create-vault.png)

    O **cofres dos serviços de recuperação** é aberto o menu. Pede-lhe fornecer informações para **nome**, **subscrição**, **grupo de recursos**, e **localização**.

    ![Painel "Cofres dos serviços de recuperação"](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. Para o **Nome**, introduza um nome amigável para identificar o cofre. O nome tem de ser exclusivo para a subscrição do Azure. Escreva um nome que contenha, pelo menos, dois, mas não mais de 50 carateres. O nome tem de começar com uma letra e pode conter apenas letras, números e hífenes.
6. Para **subscrição**, selecione a subscrição que pretende utilizar. Se for um membro de apenas uma subscrição, será apresentada esse nome. Se não tiver a certeza de que subscrição utilizar, aceda a com a predefinição (ou sugerida) subscrição. Existem várias opções apenas se for seu trabalho ou a conta da escola associada com várias subscrições do Azure.
7. Para **grupo de recursos** pode utilizar um grupo de recursos existente ou crie um novo. Para ver a lista de grupos de recursos na sua subscrição disponíveis, selecione **utilizar existente**e clique no menu pendente. Para criar um novo grupo de recursos, selecione **criar nova** e escreva o nome. Para obter informações completas sobre os grupos de recursos, consulte [descrição geral do Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
8. Para **localização** selecione a região geográfica do cofre. Se estiver a criar um cofre para proteger máquinas virtuais, o Cofre *tem* estar na mesma região que as máquinas virtuais.

   > [!IMPORTANT]
   > Se não tiver a certeza de que a localização na qual existe a VM, feche a caixa de diálogo de criação do cofre e aceda à lista de máquinas virtuais no portal. Se tiver máquinas virtuais em várias regiões, crie um cofre dos Serviços de Recuperação em cada região. Crie o cofre na primeira localização antes de ir para a localização seguinte. Não é necessário especificar contas de armazenamento para armazenar os dados de cópia de segurança. O Cofre dos serviços de recuperação e o serviço de cópia de segurança do Azure processam que automaticamente.
   >
   >

9. Quando estiver pronto para criar o Cofre dos serviços de recuperação, clique em **criar**.

    ![Lista de cofres de cópia de segurança](./media/backup-create-rs-vault/click-create-button.png)

    Pode demorar algum tempo até que o cofre dos Serviços de Recuperação seja criado. Monitorize as notificações de estado na secção notificações (a área superior direita do portal). Depois do cofre for criado, aparecerá na lista de cofres dos serviços de recuperação. Se ainda não vir o cofre, clique em **atualizar**.

     ![Lista de cofres de cópia de segurança](./media/backup-create-rs-vault/refresh-button.png)
