---
title: Gerir recursos do Azure com o Cloud Explorer | Documentos da Microsoft
description: Saiba como utilizar o Cloud Explorer para procurar e gerir recursos do Azure no Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/25/2017
ms.author: ghogen
ms.openlocfilehash: 306ff780e500c937dbdbae307ec6c96162e4b2e7
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969343"
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>Gerir os recursos associados com suas contas do Azure no Gerenciador de nuvem do Visual Studio

Explorador de cloud permite-lhe ver os seus recursos do Azure e grupos de recursos, Inspecione as respetivas propriedades e executar ações de diagnóstico de chave do desenvolvedor de dentro do Visual Studio. 

Como o [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer baseia-se a pilha do Azure Resource Manager. Por conseguinte, o Cloud Explorer compreende recursos, tais como grupos de recursos do Azure e serviços do Azure, tais como aplicações lógicas e API apps, e oferece suporte a [controlo de acesso baseado em funções](role-based-access-control/role-assignments-portal.md) (RBAC). 

## <a name="prerequisites"></a>Pré-requisitos

- [Visual Studio 2017](https://www.visualstudio.com/downloads/) com o **carga de trabalho do Azure** selecionado, ou uma versão anterior do Visual Studio com o [Microsoft Azure SDK para .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Conta do Microsoft Azure - se não tiver uma conta, pode [Inscreva-se numa avaliação gratuita](http://go.microsoft.com/fwlink/?LinkId=623901) ou [ativar os benefícios de subscritor do Visual Studio](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Para ver o Cloud Explorer, selecione **View** > **Cloud Explorer** na barra de menus.   
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a>Adicione um Azure da conta para o Cloud Explorer

Para ver os recursos associados uma conta do Azure, primeiro tem de adicionar a conta para o Cloud Explorer. 

1. Na **Cloud Explorer**, selecione **definições de conta do Azure**.

  ![Ícone de definições de conta de Explorer Azure de cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Selecione **gerir contas**. 

  ![Ligação de adicionar a conta do Explorador de cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/manage-accounts-link.png)

1. Inicie sessão na conta do Azure cujos recursos que pretende procurar. 

1. Depois de iniciar sessão numa conta do Azure, exibem as subscrições associadas essa conta. Selecione as caixas de verificação para as subscrições de conta que pretende procurar e, em seguida, selecione **aplicar**. 
 
  ![Explorador da cloud: selecione as subscrições do Azure para apresentar](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Depois de selecionar as subscrições cujos recursos que pretende procurar, apresentam esses recursos e subscrições no Cloud Explorer.

  ![Cloud recursos Explorer listagem para uma conta do Azure](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Remover uma conta do Azure do Cloud Explorer 

1. Na **Cloud Explorer**, selecione **gestão de contas**.

  ![Ícone de definições de conta de Explorer Azure de cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Junto a conta que pretende remover, selecione **gerir contas**.

  ![Ícone de definições de conta de Explorer Azure de cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

1. Escolher **remover** para remover uma conta.

  ![Caixa de diálogo do cloud Explorer gerir contas](media/vs-azure-tools-resources-managing-with-cloud-explorer/accountmanage.PNG)

## <a name="view-resource-types-or-resource-groups"></a>Ver os tipos de recursos ou grupos de recursos

Para ver os recursos do Azure, pode escolher entre **tipos de recursos** ou **grupos de recursos** vista.

1. Na **Cloud Explorer**, selecione o menu suspenso de exibição de recurso.

  ![Lista de lista pendente do cloud Explorer para selecionar a vista de recursos pretendido](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. No menu de contexto, selecione a vista pretendida: 

    * **Tipos de recursos** visualizar - o comuns utilizados no [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), mostra os recursos do Azure, categorizados por tipo, como aplicações web, contas de armazenamento e máquinas virtuais. 
    * **Grupos de recursos** exibir - recursos do Azure categoriza pelo grupo de recursos do Azure com a qual está associados. Um grupo de recursos é um pacote de recursos do Azure, normalmente utilizadas por uma aplicação específica. Para saber mais sobre os grupos de recursos do Azure, veja [descrição geral do Azure Resource Manager](./azure-resource-manager/resource-group-overview.md).

A imagem seguinte mostra uma comparação dos modos de exibição de dois recursos:

  ![Comparação de vistas de recursos Explorer na cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Ver e navegue por recursos no Cloud Explorer

Para navegar para um recurso do Azure e ver a informação no Cloud Explorer, expanda o item tipo ou grupo de recursos associados e, em seguida, selecione o recurso. Quando seleciona um recurso, as informações são apresentadas em duas guias - **ações** e **propriedades** - na parte inferior do Cloud Explorer. 

- **Ações** separador – apresenta uma lista de ações que pode realizar no Cloud Explorer para o recurso selecionado. Também pode ver estas opções clicando com o recurso para exibir o menu de contexto.

- **Propriedades** separador – mostra as propriedades do recurso, como o seu tipo, região e grupo de recursos com a qual está associado.

A imagem seguinte mostra uma comparação de exemplo do que vê em cada separador, para um serviço de aplicações:

  ![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Cada recurso tem a ação **abrir no portal**. Ao escolher esta ação, o Cloud Explorer apresenta o recurso selecionado no [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). O **abrir no portal** recurso é útil para navegar para recursos profundamente aninhados.

Ações adicionais e valores de propriedade também podem aparecer com base no recurso do Azure. Por exemplo, aplicações web e o logic apps também tem as ações **aberto no browser** e **anexar o depurador** além **abrir no portal**. As ações para abrir os editores aparecem ao escolher um blob da conta de armazenamento, uma fila ou uma tabela. As aplicações do Azure têm **URL** e **estado** propriedades, enquanto os recursos de armazenamento têm as propriedades de cadeia de ligação e a chave.

## <a name="find-resources-in-cloud-explorer"></a>Encontre recursos no Cloud Explorer

Para localizar recursos com um nome específico nas suas subscrições de conta do Azure, introduza o nome na **pesquisa** caixa no Cloud Explorer.

  ![Localizar recursos no Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

À medida que introduzir carateres da **pesquisa** caixa, apenas os recursos que correspondam a esses caracteres são apresentados na árvore de recursos.
