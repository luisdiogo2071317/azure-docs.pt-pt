---
title: Gerir contas de laboratório nos serviços de laboratório do Azure | Microsoft Docs
description: Saiba como criar uma conta de laboratório, ver todas as contas de laboratório ou eliminar uma conta de laboratório numa subscrição do Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 1286698fb7dd13c7568a0fa8b20c50511d5a6919
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Gerir contas de laboratório nos serviços de laboratório do Azure (anteriormente denominado Azure DevTest Labs)
Este artigo descreve como criar uma conta de laboratório, ver todas as contas de laboratório ou eliminar uma conta de laboratório.

## <a name="create-a-lab-account"></a>Criar uma conta de laboratório
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu principal do lado esquerdo, selecione **Criar um recurso**.
3. Procurar por **Serviços de Laboratório** no Azure Marketplace e selecione **Serviços de Laboratório** na lista pendente. 
4. Selecione **laboratório Sercices (pré-visualização)** na lista flitered de serviços. 
5. Na janela **Criar uma conta de laboratório**, selecione **Criar**.
7. Na janela **Conta de laboratório**, realize as ações seguintes: 
    1. Em **Nome da conta de laboratório**, introduza um nome. 
    2. Selecione a **Subscrição do Azure**, na qual pretende criar a conta de laboratório.
    3. Em **Grupo de recursos**: selecione **Criar novo** e introduza um nome para o grupo de recursos.
    4. Em **Localização**, selecione uma região/localização na qual pretende criar a conta de laboratório. 
    5. Selecione **Criar**. 

        ![Janela Criar uma conta de laboratório](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Se não vir a página da conta de laboratório, selecione o botão **notificações** e, em seguida, clique no botão **Ir para recurso** nas notificações. 

    ![Janela Criar uma conta de laboratório](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Verá a página **conta de laboratório** seguinte:

    ![Página da conta de laboratório](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Adicionar um utilizador à função de Criador do Laboratório
Para dar permissão aos educadores para criarem laboratórios para as classes deles, adicione-os à função de Criador do Laboratório:

1. Na página **Conta de Laboratório**, selecione **Controlo de acesso (IAM)** e clique em **+ Adicionar** na barra de ferramentas. 

    ![Página da conta de laboratório](./media/tutorial-setup-lab-account/access-control.png)
2. Na página **Adicionar permissões**, selecione **Criador do Laboratório** em **Função**, selecione o utilizador que quer adicionar à função de Criadores do Laboratório e selecione **Guardar**. 

    ![Adicionar utilizador à função de Criador do Laboratório](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="view-lab-accounts"></a>Contas de laboratório de vista
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os recursos** no menu. 
3. Selecione **laboratório serviços** para o **tipo**. 
    Também pode filtrar por subscrição, o grupo de recursos, localizações e as etiquetas. 

## <a name="delete-a-lab-account"></a>Eliminar uma conta de laboratório
Siga as instruções da secção anterior, que apresenta as contas de laboratório numa lista. Utilize as seguintes instruções para eliminar uma conta de laboratório: 

1. Selecione o **conta laboratório** que pretende eliminar. 
2. Selecione **eliminar** da barra de ferramentas. 
3. Tipo **Sim** confirmação.
4. Selecione **Eliminar**. 

## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](tutorial-setup-classroom-lab.md)
- [Configurar um laboratório personalizado](tutorial-create-custom-lab.md)
