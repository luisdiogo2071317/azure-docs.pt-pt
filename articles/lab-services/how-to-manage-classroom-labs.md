---
title: Gerir laboratórios de sala de aulas nos serviços de laboratório do Azure | Microsoft Docs
description: Saiba como criar e configurar um laboratório de sala de aula, ver todos os os sala de aulas laboratórios, shre o registo de ligação com um utilizador de laboratório ou eliminar um laboratório.
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
ms.openlocfilehash: 17544275f921486529518e37eb171cd0b4f26791
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="manage-classroom-labs-in-azure-lab-services-formerly-azure-devtest-labs"></a>Gerir laboratórios de sala de aulas nos serviços de laboratório do Azure (anteriormente denominado Azure DevTest Labs)
Este artigo descreve como criar e configurar um laboratório de sala de aula, ver todos os laboratórios de sala de aula ou eliminar um laboratório de sala de aula.

## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aulas

1. Navegue para [Web site Azure laboratório Services](https://labs.azure.com).
2. No **novo laboratório** janela, efetue as seguintes ações: 
    1. Especifique um **nome** para o laboratório de sala de aula. 
    2. Selecione o **tamanho** da máquina virtual que planeia utilizar a sala de aula.
    3. Selecione o **imagem** a utilizar para criar a máquina virtual.
    4. Especifique o **as credenciais predefinidas** a utilizar para iniciar sessão nas máquinas virtuais no laboratório.
    7. Selecione **Guardar**.

        ![Criar um laboratório de sala de aulas](./media/how-to-manage-classroom-labs/new-lab-window.png)
1. Pode ver o **home page do** para o laboratório. 
    
    ![Home page da sala de aulas laboratório](./media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Configurar a política de utilização

1. Selecione **política de utilização**. 
2. No **política de utilização**, definições, introduza o **número de utilizadores que** permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](./media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configurar o modelo
Um modelo num laboratório é de que as máquinas virtuais de todos os utilizadores são criadas. Configure a máquina virtual do modelo, de modo a que está configurado com exatamente o que pretende fornecer aos utilizadores laboratório. Pode fornecer um nome e descrição do modelo que veem os utilizadores de laboratório e definir a visibilidade como "Público" para disponibilizar as instâncias do modelo de VM para os utilizadores de laboratório.  

## <a name="set-template-title-and-description"></a>Título de modelo do conjunto e a descrição
1. No **modelo** secção, selecione **editar** (ícone de lápis) para o modelo. 
2. No **vista do utilizador** janela, introduza um **título** para o modelo.
3. Introduza **Descrição** para o modelo.
4. Selecione **Guardar**.

    ![Descrição de laboratório de sala de aulas](./media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Tornar as instâncias do modelo de público 
Depois de definir a visibilidade de um modelo para **pública**, serviços de laboratório do Azure cria VMs no laboratório utilizando o modelo. O número de VMs criados neste processo é os mesmos que o número máximo de utilizadores com permissão para o laboratório, que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração como modelo.  

1. Selecione **visibilidade** no **modelo** secção. 
2. No **disponibilidade** página, selecione **pública**.
    
    > [!IMPORTANT]
    > Depois de um modelo é publicamente disponível, não é possível alterar o acesso privado. 
3. Selecione **Guardar**.

    ![Disponibilidade](./media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para estudantes

1. Selecione **registo de utilizador** mosaico.
2. No **registo de utilizador** caixa de diálogo, selecione o **cópia** botão. A ligação é copiada para a área de transferência. Cole-o num editor de e-mail e envie um e-mail para o estudante. 

    ![Ligação de registo do estudante](./media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Ver todos os laboratórios de sala de aulas
1. Navegue para [portal dos serviços do Azure laboratório](https://labs.azure.com).
2. Confirme que vê todos os laboratórios na conta de laboratório selecionado. 

    ![Todos os laboratórios](./media/how-to-manage-classroom-labs/all-labs.png)
3. Utilize na lista pendente na parte superior para selecionar uma conta diferente de laboratório. Consulte laboratórios na conta de laboratório selecionado. 

## <a name="delete-a-classroom-lab"></a>Eliminar um laboratório de sala de aulas
1. No mosaico para o laboratório, selecione as reticências (…) no canto. 

    ![Selecione o laboratório](./media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecione **Eliminar**. 

    ![Eliminar botão](./media/how-to-manage-classroom-labs/delete-button.png)
3. No **eliminar laboratório** caixa de diálogo, selecione **eliminar**. 

    ![Eliminar a caixa de diálogo](./media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Passos Seguintes
Introdução ao configurar um laboratório utilizando os serviços de laboratório do Azure:

- [Configurar um laboratório de sala de aulas](how-to-manage-classroom-labs.md)
- [Configurar um laboratório personalizado](tutorial-create-custom-lab.md)
