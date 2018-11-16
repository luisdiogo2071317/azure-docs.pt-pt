---
title: Gerir contas de laboratório no Azure Lab Services | Documentos da Microsoft
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
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: c672634e79cbc0850edfe7f7fbdb0a880d5cf0d8
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707134"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Gerir contas de laboratório no Azure Lab Services 
No Azure Lab Services, uma conta de laboratório é um contentor para laboratórios geridos, como laboratórios de sala de aula. Um administrador configura uma conta de laboratório com o Azure Lab Services e fornece acesso a proprietários de laboratório que podem criar laboratórios na conta. Este artigo descreve como criar uma conta de laboratório, ver todas as contas de laboratório ou eliminar uma conta de laboratório.

## <a name="create-a-lab-account"></a>Criar uma conta de laboratório
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu principal do lado esquerdo, selecione **Criar um recurso**.
3. Procurar por **Serviços de Laboratório** no Azure Marketplace e selecione **Serviços de Laboratório** na lista pendente. 
4. Selecione **Serviços de Laboratório (Pré-visualização)** na lista filtrada de serviços. 
5. Na janela **Criar uma conta de laboratório**, selecione **Criar**.
7. Na janela **Conta de laboratório**, realize as ações seguintes: 
    1. Em **Nome da conta de laboratório**, introduza um nome. 
    2. Selecione a **Subscrição do Azure**, na qual pretende criar a conta de laboratório.
    3. Em **Grupo de recursos**: selecione **Criar novo** e introduza um nome para o grupo de recursos.
    4. Em **Localização**, selecione uma região/localização na qual pretende criar a conta de laboratório. 
    5. Selecione **Criar**. 

        ![Janela Criar uma conta de laboratório](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Se não vir a página da conta de laboratório, selecione o botão **notificações** e, em seguida, clique no botão **Ir para recurso** nas notificações. 

    ![Janela Criar uma conta de laboratório](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Verá a página **conta de laboratório** seguinte:

    ![Página da conta de laboratório](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Adicionar um utilizador à função de Criador do Laboratório
Para configurar um laboratório de sala de aula numa conta de laboratório, o utilizador tem de ser membro da função **Criador de Laboratório** na conta de laboratório. A conta que utilizou para criar a conta de laboratório é adicionada automaticamente a esta função. Se estiver a planear utilizar a mesma conta de utilizador para criar um laboratório de sala de aula, pode ignorar este passo. Para utilizar outra conta de utilizador para criar um laboratório de sala de aula, siga os passos abaixo: 

1. Na página **Conta de Laboratório**, selecione **Controlo de acesso (IAM)** e clique em **+ Adicionar** na barra de ferramentas. 

    ![Página da conta de laboratório](../media/tutorial-setup-lab-account/access-control.png)
2. Na página **Adicionar permissões**, selecione **Criador do Laboratório** em **Função**, selecione o utilizador que quer adicionar à função de Criadores do Laboratório e selecione **Guardar**. 

    ![Adicionar utilizador à função de Criador do Laboratório](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Especifique as imagens do Marketplace disponíveis para os proprietários de laboratório
Enquanto proprietário de uma conta de laboratório, pode especificar as imagens do Marketplace que os criadores de laboratórios podem utilizar para criar laboratórios nessa conta. 

1. Selecione **Imagens do Marketplace** no menu à esquerda. Por predefinição, verá a lista completa de imagens (ativadas e desativadas). Pode filtrar a lista para ver apenas as imagens ativadas/desativadas ao selecionar a opção **Apenas ativado**/**Apenas desativado** na lista pendente na parte superior. 
    
    ![Página de imagens do Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    As imagens do Marketplace que aparecem na lista são as que satisfazem as seguintes condições:
        
    - Cria uma VM individual.
    - Utiliza o Azure Resource Manager para aprovisionar VMs
    - Não requer a compra de planos de licenciamento extra
2. Para **desativar** uma imagem do Marketplace que tenha sido ativada, efetue uma das seguintes ações: 
    1. Selecione **... (reticências)** na última coluna e selecione **Desativar imagem**. 

        ![Desativar uma imagem](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Selecione uma ou mais imagens a partir da lista ao selecionar as caixas de verificação antes dos nomes de imagem na lista e selecione **Desativar imagens selecionadas**. 

        ![Desativar várias imagens](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. De forma semelhante, para **ativar** uma imagem do Marketplace, efetue uma das seguintes ações: 
    1. Selecione **... (reticências)** na última coluna e selecione **Ativar imagem**. 
    2. Selecione uma ou mais imagens a partir da lista ao selecionar as caixas de verificação antes dos nomes de imagem na lista e selecione **Ativar imagens selecionadas**. 

## <a name="view-lab-accounts"></a>Contas de laboratório do Vista
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os recursos** no menu. 
3. Selecione **Lab Services** para o **tipo**. 
    Também pode filtrar por subscrição, grupo de recursos, localizações e as etiquetas. 

## <a name="delete-a-lab-account"></a>Eliminar uma conta de laboratório
Siga as instruções da secção anterior, que apresenta as contas de laboratório numa lista. Utilize as instruções seguintes para eliminar uma conta de laboratório: 

1. Selecione o **conta de laboratório** que pretende eliminar. 
2. Selecione **eliminar** da barra de ferramentas. 
3. Tipo **Sim** confirmação.
4. Selecione **Eliminar**. 

## <a name="view-and-manage-labs-in-the-lab-account"></a>Ver e gerir laboratórios na conta de laboratório

1. Sobre o **conta de laboratório** página, selecione **laboratórios** no menu da esquerda.

    ![Laboratórios na conta](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Verá um **lista de laboratórios** na conta com as seguintes informações: 
    1. Nome do laboratório.
    2. A data em que foi criado o laboratório. 
    3. Endereço de e-mail do utilizador que criou o laboratório. 
    4. Número máximo de utilizadores com permissão para o laboratório. 
    5. Estado do laboratório. 

## <a name="delete-a-lab-in-the-lab-account"></a>Eliminar um laboratório na conta de laboratório
Siga as instruções na secção anterior para ver uma lista dos laboratórios na conta de laboratório.

1. Selecione **... (reticências)** e selecione **eliminar**. 

    ![Eliminar um laboratório – botão](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Selecione **Sim** na mensagem de aviso. 



## <a name="next-steps"></a>Passos Seguintes
Consulte os seguintes artigos:

- [Como proprietário de um laboratório, criar e gerir laboratórios](how-to-manage-classroom-labs.md)
- [Como proprietário de um laboratório, configurar e publicar modelos](how-to-create-manage-template.md)
- [Como proprietário de um laboratório, configurar e controlar a utilização de um laboratório](how-to-configure-student-usage.md)
- [Como um utilizador de laboratório, acessar os laboratórios de sala de aula](how-to-use-classroom-lab.md)