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
ms.date: 04/20/2018
ms.author: spelluru
ms.openlocfilehash: 53494abead5701052f6e08f68b01ccdf1bfa211c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="manage-lab-accounts-in-azure-lab-services-formerly-azure-devtest-labs"></a>Gerir contas de laboratório nos serviços de laboratório do Azure (anteriormente denominado Azure DevTest Labs)
Este artigo descreve como criar uma conta de laboratório, ver todas as contas de laboratório ou eliminar uma conta de laboratório.

## <a name="create-a-lab-account"></a>Criar uma conta de laboratório
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu principal no lado esquerdo, selecione **crie um recurso**.
3. Procurar **laboratório serviços** no Azure Marketplace e selecione **laboratório serviços** na lista pendente. 
4. Selecione **laboratório Sercices (pré-visualização)** na lista flitered de serviços. 
5. No **criar uma conta de laboratório** janela, selecione **criar**.
7. No **conta laboratório** janela, efetue as seguintes ações: 
    1. Para **nome da conta de laboratório**, introduza um nome. 
    2. Selecione o **subscrição do Azure** no qual pretende criar a conta de laboratório.
    3. Para **grupo de recursos**, selecione **criar nova**e introduza um nome para o grupo de recursos.
    4. Para **localização**, selecione uma região/localização em que pretenda que a conta de laboratório a ser criado. 
    5. Selecione **Criar**. 

        ![Criar uma janela de conta de laboratório](./media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Se não vir a página para a conta de laboratório, selecione o **notificações** botão e, em seguida, clique em **aceda a recursos** botão nas notificações. 

    ![Criar uma janela de conta de laboratório](./media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Consulte o seguinte **conta laboratório** página:

    ![Página de conta de laboratório](./media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Adicionar um utilizador à função de criador de laboratório
Para fornecer educators a permissão para criar laboratórios para as respetivas classes, adicioná-los para a função de criador de laboratório:

1. No **laboratório conta** página, selecione **(IAM) do controlo de acesso**e clique em **+ adicionar** na barra de ferramentas. 

    ![Página de conta de laboratório](./media/tutorial-setup-lab-account/access-control.png)
2. No **adicionar permissões** página, selecione **criador de laboratório** para **função**, selecione o utilizador que pretende adicionar à função de criadores de laboratório e selecione **guardar**. 

    ![Adicionar utilizador à função de criador de laboratório](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


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
Introdução ao configurar um laboratório utilizando os serviços de laboratório do Azure:

- [Configurar um laboratório de sala de aulas](tutorial-setup-classroom-lab.md)
- [Configurar um laboratório personalizado](tutorial-create-custom-lab.md)
