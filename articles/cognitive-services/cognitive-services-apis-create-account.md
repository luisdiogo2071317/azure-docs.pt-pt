---
title: Criar uma conta de APIs serviços cognitivos no portal do Azure | Documentos da Microsoft
description: Como criar uma conta de APIs serviços cognitivos da Microsoft no portal do Azure.
services: cognitive-services
documentationcenter: ''
author: garyericson
manager: cgronlun
editor: ''
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: cognitive-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: garye
ms.reviewer: gibattag
ms.openlocfilehash: ed5f19b23375ecb83e19274c7405e9a1208a7985
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036161"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Criar uma conta de APIs serviços cognitivos no portal do Azure

Para utilizar as APIs serviços cognitivos da Microsoft, tem primeiro de criar uma conta no portal do Azure.

1. Inicie sessão no [portal do Azure](http://portal.azure.com).

2. Clique em **+ criar um recurso**.

3. No Azure Marketplace, selecione **IA + serviços cognitivos** e detetar a lista de APIs disponíveis. Clique em **ver todas as** para ver a lista completa de APIs serviços cognitivos. Clique na API à sua escolha para continuar.

    ![Selecione as APIs serviços cognitivos](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Sobre o **criar** página, forneça as seguintes informações:

   - **Nome da conta:** nome da conta. Recomendamos que utilize um nome descritivo, por exemplo *AFaceAPIAccount*.

   - **Subscrição:** Selecione uma das subscrições do Azure disponíveis na qual tenha, pelo menos, permissões de contribuinte.

   - **Tipo de API:** escolha a API de serviços cognitivos que pretende utilizar. Para obter mais informações sobre as várias APIs serviços cognitivos disponíveis, visite o [dos serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Escalão de preço:** o custo da sua conta dos serviços cognitivos depende da utilização real e das opções que escolher. Para obter mais informações sobre os preços para cada API, consulte a [páginas de preços](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Grupo de recursos:** um grupo de recursos é uma coleção de recursos que partilham o mesmo ciclo de vida, permissões e políticas. Para saber mais sobre grupos de recursos, veja [recursos de gerir o Azure através do portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Localização do grupo de recursos:** isto é necessário apenas se a API selecionada é global (não vinculado a uma localização). Se a API é global e não está vinculado a uma localização, no entanto, tem de especificar uma localização para o grupo de recursos onde os metadados associados a conta de API dos serviços cognitivos residem. Esta localização não tem impacto sobre a disponibilidade de tempo de execução da sua conta. Para saber mais sobre o grupo de recursos, veja [recursos de gerir o Azure através do portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Confirmação explícita dos termos de serviço Online:** para criar uma conta, proprietários de subscrições ou contribuidores (conforme definido pela [controlo de acesso](https://docs.microsoft.com/azure/role-based-access-control/overview)) tem de confirmar explicitamente os termos que se aplicam aos serviços cognitivos no [termos de serviço Online](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     A proprietário da subscrição pode desativar a criação de conta dos serviços cognitivos para um grupo de recursos específico ou a assinatura por meio [políticas do Azure](../azure-policy/azure-policy-introduction.md) seguindo o artigo [portal de utilizar o Azure para atribuir e gerir políticas de recursos](../azure-policy/assign-policy-definition.md) e atribuir uma definição de política de "tipos de recurso não permitidos" e especificando **Microsoft.CognitiveServices/accounts** como o tipo de recurso de destino.

     Se a criação da conta foi desativada, o seguinte erro seria exibido no momento da criação da conta:

     ![Erro ao criar conta](media/cognitive-services-apis-create-account/error-message.png)

5. Para afixar a conta ao dashboard do portal do Azure, clique em **afixar ao Dashboard**.

6. Clique em **Criar** para criar a conta.

Depois da conta dos serviços cognitivos é implementada com êxito, clique no mosaico no dashboard para ver as informações de conta.

Pode utilizar o **URL de ponto final** no **descrição geral** secção e chaves no **chaves** secção para começar a criar API chama-se nas suas aplicações.

![Apresentar informações de conta](media/cognitive-services-apis-create-account/display-account.png)

![Exibir as chaves de conta](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Próximos Passos

Para obter mais informações sobre todos os serviços cognitivos da Microsoft disponíveis, consulte [dos serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/).

Para guias de início rápido para usando algumas APIs serviços cognitivos de exemplo:

 - [Inícios rápidos do computador visão c#](computer-vision/quickstarts/csharp.md)
 - [Análise de texto com Python](text-analytics/quickstarts/python.md)
 - [Com o JavaScript, a API face](face/quickstarts/javascript.md)
