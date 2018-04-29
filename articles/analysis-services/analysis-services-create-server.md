---
title: Criar um servidor de Analysis Services no Azure | Microsoft Docs
description: Saiba como criar uma instância de servidor do Analysis Services no Azure.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c49e886ee5b980e8fd059d72eb2e4a3f0dc895c4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-an-analysis-services-server-in-azure-portal"></a>Criar um servidor de Analysis Services no portal do Azure
Este artigo explica como criar um recurso de servidor do Analysis Services na sua subscrição do Azure.

Antes de começar, tem de: 

* **Subscrição do Azure**: aceda à [Avaliação Gratuita do Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) para criar uma conta.
* **Azure Active Directory**: sua subscrição tem de ser associada a um inquilino do Azure Active Directory. E, terá de iniciar sessão no Azure com uma conta no diretório do Azure Active Directory. Para saber mais,v eja [Authentication and user permissions](analysis-services-manage-users.md) (Autenticação e permissões de utilizador).

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure 

Inicie sessão no [Portal do Azure](https://portal.azure.com)


## <a name="create-a-server"></a>Criar um servidor

1. Clique em **+ criar um recurso** > **dados + análise** > **Analysis Services**.

    ![Portal](./media/analysis-services-create-server/aas-create-server-portal.png)

2. No **Analysis Services**, preencha os campos obrigatórios e, em seguida, prima **criar**.
   
    ![Criar servidor do](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Nome do servidor**: escreva um nome exclusivo utilizado para referenciar o servidor.
   * **Subscrição**: selecione a subscrição deste servidor será associado.
   * **Grupo de recursos**: criar um novo grupo de recursos ou selecione uma que já tem. Grupos de recursos foram concebidos para ajudar a gerir uma coleção de recursos do Azure. Para obter mais informações, consulte [grupos de recursos](../azure-resource-manager/resource-group-overview.md).
   * **Localização**: localização de centro de dados do Azure este aloja o servidor. Escolha uma localização mais próximo da sua base de utilizadores maior.
   * **Escalão de preço**: selecione um escalão de preço. Se estiver a testar e pretende instalar a base de dados do modelo de exemplo, selecione o livre **D1** camada. Para obter mais informações, consulte [preços do Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/). 
    * **Administrador**: por predefinição, esta será a conta que tem sessão iniciada com. Pode escolher uma conta diferente do Azure Active Directory.
    * **Definição de armazenamento de cópia de segurança**: opcional. Se já tiver um [conta de armazenamento](../storage/common/storage-introduction.md), pode especificá-la como predefinição para cópia de segurança do modelo da base de dados. Também pode especificar [cópia de segurança e restaurar](analysis-services-backup.md) definições mais tarde.
    * **Expiração de chave de armazenamento**: opcional. Especificar um período de expiração de chave de armazenamento.
3. Clique em **Criar**.

Criar normalmente tem num minuto. Se tiver selecionado **adicionar ao Portal**, navegue até ao seu portal para ver o novo servidor. Ou, navegue para **todos os serviços** > **Analysis Services** para ver se o servidor está pronto.

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário, elimine o servidor. No seu servidor **descrição geral**, clique em **eliminar**. 

 ![Limpeza](./media/analysis-services-create-server/aas-create-server-cleanup.png)


## <a name="next-steps"></a>Passos Seguintes

[Adicionar um modelo de dados de exemplo](analysis-services-create-sample-model.md) ao seu servidor.  
[Instale um gateway de dados no local](analysis-services-gateway-install.md) se o modelo de dados liga-se a origens de dados no local.  
[Implementar um projeto de modelo em tabela](analysis-services-deploy.md) do Visual Studio.   


