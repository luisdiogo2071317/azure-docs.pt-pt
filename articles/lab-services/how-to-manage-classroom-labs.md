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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: e388045e839c19c68ad2c00f31d74c73e107872c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="manage-classroom-labs-in-azure-lab-services-formerly-azure-devtest-labs"></a>Gerir laboratórios de sala de aulas nos serviços de laboratório do Azure (anteriormente denominado Azure DevTest Labs)
Este artigo descreve como criar e configurar um laboratório de sala de aula, ver todos os laboratórios de sala de aula ou eliminar um laboratório de sala de aula.

## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aula

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com).
2. Na janela **Novo Laboratório**, realize as ações abaixo: 
    1. Especifique um **nome** para o laboratório de sala de aula. 
    2. Selecione o **tamanho** da máquina virtual que planeia utilizar na sala de aula.
    3. Selecione a **imagem** a utilizar para criar a máquina virtual.
    4. Especifique o **as credenciais predefinidas** a utilizar para iniciar sessão nas máquinas virtuais no laboratório.
    7. Selecione **Guardar**.

        ![Criar um laboratório de sala de aula](./media/how-to-manage-classroom-labs/new-lab-window.png)
1. Verá a **home page** do laboratório. 
    
    ![Home page do laboratório da sala de aula](./media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Configurar a política de utilização

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](./media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configurar o modelo
Um modelo num laboratório é uma imagem de base máquina virtual a partir da qual as máquinas virtuais de todos os utilizadores são criadas. Configure a máquina virtual do modelo para que seja configurado com exatamente o que pretende fornecer aos utilizadores do laboratório. Pode fornecer um nome e descrição do modelo que veem os utilizadores de laboratório. Defina a visibilidade do modelo ao público para disponibilizar as instâncias do modelo de VM para os utilizadores de laboratório.  

### <a name="set-template-title-and-description"></a>Título de modelo do conjunto e a descrição
1. Na secção **Modelo**, selecione **Editar** (ícone de lápis) para o modelo. 
2. Na janela **Vista do utilizador**, introduza um **título** para o modelo.
3. Introduza a **descrição** do modelo.
4. Selecione **Guardar**.

    ![Descrição do laboratório da sala de aula](./media/how-to-manage-classroom-labs/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Tornar públicas as instâncias do modelo 
Depois de definir a visibilidade de um modelo como **Pública**, o Azure Lab Services cria VMs no laboratório através do modelo. O número de VMs criados neste processo é igual ao número máximo de utilizadores permitido para o laboratório, que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo.  

1. Selecione **Visibilidade** na secção **Modelo**. 
2. Na página **Disponibilidade**, selecione **Pública**.
    
    > [!IMPORTANT]
    > Depois de um modelo estar disponível publicamente, não é possível alterar o acesso para privado. 
3. Selecione **Guardar**.

    ![Disponibilidade](./media/how-to-manage-classroom-labs/public-access.png)

## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Selecione o mosaico **Registo de utilizador**.
2. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. 

    ![Ligação de registo do aluno](./media/how-to-manage-classroom-labs/registration-link.png)

## <a name="view-all-classroom-labs"></a>Ver todos os laboratórios de sala de aulas
1. Navegue para [portal dos serviços do Azure laboratório](https://labs.azure.com).
2. Confirme que vê todos os laboratórios na conta de laboratório selecionado. 

    ![Todos os laboratórios](./media/how-to-manage-classroom-labs/all-labs.png)
3. Utilize na lista pendente na parte superior para selecionar uma conta diferente de laboratório. Consulte laboratórios na conta de laboratório selecionado. 

## <a name="delete-a-classroom-lab"></a>Eliminar um laboratório de sala de aulas
1. No mosaico para o laboratório, selecione as reticências (…) no canto. 

    ![Selecionar o laboratório](./media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecione **Eliminar**. 

    ![Eliminar botão](./media/how-to-manage-classroom-labs/delete-button.png)
3. No **eliminar laboratório** caixa de diálogo, selecione **eliminar**. 

    ![Eliminar a caixa de diálogo](./media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 

## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório personalizado](tutorial-create-custom-lab.md)
