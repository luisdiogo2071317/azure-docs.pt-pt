---
title: Configurar uma conta de laboratório com o Azure Lab Services | Microsoft Docs
description: Neste tutorial, vai configurar uma conta de laboratório com o Azure Lab Services (anteriormente DevTest Labs).
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 22a9e90404475e8ff1f1ea72c233b1abfed938f6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361388"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services-formerly-azure-devtest-labs"></a>Tutorial: Configurar uma conta de laboratório com o Azure Lab Services (anteriormente Azure DevTest Labs)
Neste tutorial, vai atuar como um administrador de laboratório para criar uma conta de laboratório com o Azure Lab Services. Em seguida, vai dar permissão aos educadores para criarem laboratórios para as classes deles nesta conta de laboratório. Um educador pode configurar um laboratório para uma classe, através do [site do Azure Lab Services](https://labs.azure.com).   

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Criar uma conta de laboratório
> * Adicionar um utilizador à função de Criador do Laboratório

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-a-lab-account"></a>Criar uma conta de laboratório
Os passos seguintes mostram como utilizar o portal do Azure para criar uma conta de laboratório com o Azure Lab Services. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu principal do lado esquerdo, selecione **Criar um recurso**.
3. Procurar por **Serviços de Laboratório** no Azure Marketplace e selecione **Serviços de Laboratório** na lista pendente. 
4. Selecione **Serviços de Laboratório (Pré-visualização)** na lista filtrada de serviços. 
1. Na janela **Criar uma conta de laboratório**, selecione **Criar**.
2. Na janela **Conta de laboratório**, realize as ações seguintes: 
    1. Em **Nome da conta de laboratório**, introduza um nome. 
    2. Selecione a **Subscrição do Azure**, na qual pretende criar a conta de laboratório.
    3. Em **Grupo de recursos**: selecione **Criar novo** e introduza um nome para o grupo de recursos.
    4. Em **Localização**, selecione uma região/localização na qual pretende criar a conta de laboratório. 
    5. Selecione **Criar**. 

        ![Janela Criar uma conta de laboratório](./media/tutorial-setup-lab-account/lab-account-settings.png)
5. Se não vir a página da conta de laboratório, selecione o botão **notificações** e, em seguida, clique no botão **Ir para recurso** nas notificações. 

    ![Janela Criar uma conta de laboratório](./media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Verá a página **conta de laboratório** seguinte:

    ![Página da conta de laboratório](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Adicionar um utilizador à função de Criador do Laboratório
Para dar permissão aos educadores para criarem laboratórios para as classes deles, adicione-os à função de Criador do Laboratório:

1. Na página **Conta de Laboratório**, selecione **Controlo de acesso (IAM)** e clique em **+ Adicionar** na barra de ferramentas. 

    ![Página da conta de laboratório](./media/tutorial-setup-lab-account/access-control.png)
2. Na página **Adicionar permissões**, selecione **Criador do Laboratório** em **Função**, selecione o utilizador que quer adicionar à função de Criadores do Laboratório e selecione **Guardar**. 

    ![Adicionar utilizador à função de Criador do Laboratório](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, criou uma conta de laboratório. Para saber mais sobre como criar um laboratório de sala de aula como uma profissão, avance para o próximo tutorial:

> [!div class="nextstepaction"]
> [Configurar um laboratório de sala de aula](tutorial-setup-classroom-lab.md)

