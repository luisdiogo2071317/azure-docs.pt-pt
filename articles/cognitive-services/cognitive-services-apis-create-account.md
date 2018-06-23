---
title: Criar uma conta de APIs de serviços cognitivos no portal do Azure | Microsoft Docs
description: Como criar uma conta Microsoft cognitivos APIs de serviços no portal do Azure.
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
ms.openlocfilehash: 3697dd0628f0028cb486139e92c032f0d757c6ed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352694"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Criar uma conta de APIs de serviços cognitivos no portal do Azure

Para utilizar o Microsoft cognitivos APIs de serviço, terá primeiro de criar uma conta no portal do Azure.

1. Inicie sessão no [portal do Azure](http://portal.azure.com).

2. Clique em **+ criar um recurso**.

3. No Azure Marketplace, selecione **AI + serviços cognitivos** e detetar a lista das APIs disponíveis. Clique em **ver todos os** para ver a lista completa das APIs de serviços cognitivos. Clique na API à sua escolha para continuar.

    ![Selecione as APIs de serviços cognitivos](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. No **criar** página, forneça as seguintes informações:

   - **Nome da conta:** nome da conta. Recomendamos que utilize um nome descritivo, por exemplo *AFaceAPIAccount*.

   - **Subscrição:** Selecione uma das subscrições do Azure disponíveis na qual tenha, pelo menos, permissões de contribuinte.

   - **Tipo de API:** escolher a API de serviços cognitivos que pretende utilizar. Para obter mais informações sobre as diversas cognitivos serviços APIs disponíveis, visite o [serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Escalão de preço:** o custo da sua conta de serviços cognitivos depende a utilização real e as opções que escolher. Para obter mais informações sobre preços para cada API, consulte o [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Grupo de recursos:** um grupo de recursos é uma coleção de recursos que partilham o mesmo ciclo de vida, permissões e as políticas. Para obter mais informações sobre grupos de recursos, consulte [recursos do Azure de gerir através do portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Localização do grupo de recursos:** isto é necessário apenas se a API selecionada é global (não vinculado a uma localização). Se a API é global e não está vinculado a uma localização, no entanto, tem de especificar uma localização para o grupo de recursos onde residem os metadados associados a conta de API de serviços cognitivos. Esta localização não tem impacto na disponibilidade de tempo de execução da sua conta. Para saber mais sobre o grupo de recursos, consulte o artigo [recursos do Azure de gerir através do portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Confirmação explícita dos termos de serviço Online:** para criar uma conta, os proprietários da subscrição ou contribuintes (conforme definido pelo [controlo de acesso em funções do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)) tem de confirmar explicitamente os termos de licenciamento que aplicar a serviços cognitivos no [termos de serviço Online](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     A proprietário da subscrição, pode desativar a criação de conta de serviços cognitivos para um grupo de recursos específico ou de subscrição através de [políticas do Azure](../azure-policy/azure-policy-introduction.md) seguindo o artigo [portal do Azure utilizando atribuir e gerir as políticas de recursos](../azure-policy/assign-policy-definition.md) e atribuir uma definição de política "tipos de recurso não permitido" e especificar **Microsoft.CognitiveServices/accounts** como o tipo de recurso de destino.

     Se a criação da conta foi desativada, será apresentado o erro seguinte no momento da criação da conta:

     ![Erro de criação de conta](media/cognitive-services-apis-create-account/error-message.png)

5. Para afixar a conta para o dashboard do portal do Azure, clique em **afixar ao Dashboard**.

6. Clique em **Criar** para criar a conta.

Depois da conta de serviços cognitivos é implementada com êxito, clique no mosaico no dashboard para ver as informações de conta.

Pode utilizar o **URL de ponto final** no **descrição geral** secção e chaves no **chaves** secção para iniciar a API de fazer chamadas nas suas aplicações.

![Apresentar informações de conta](media/cognitive-services-apis-create-account/display-account.png)

![Apresentar as chaves de conta](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Próximos Passos

Para obter mais informações sobre todos os Microsoft cognitivos serviços disponíveis, consulte [serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/).

Para guias de início rápido para alguns exemplos cognitivos APIs de serviços a utilizar:

 - [Computador visão c# inícios rápidos](/computer-vision/quickstarts/csharp.md)
 - [Análise de texto com o Python](/text-analytics/quickstarts/python.md)
 - [Enfrentam API com JavaScript](/face/quickstarts/javascript.md)
