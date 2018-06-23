---
title: Como configurar um serviço Maker de QnA - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como configurar um serviço de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce452dd686529e017b4eae4717eadb044b389409
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354068"
---
# <a name="create-a-qna-maker-service"></a>Criar um serviço de QnA Maker

Antes de poder criar quaisquer bases de dados de conhecimento Maker de QnA, tem de configurar um serviço de QnA Maker no Azure. Qualquer pessoa com autorização para criar novos recursos numa subscrição pode configurar um serviço de QnA Maker.

Esta configuração implementa alguns recursos do Azure. Em conjunto, estes recursos gerir o conteúdo da base de dados de conhecimento e fornecem capacidades de responder a pergunta, embora um ponto final.

1. Inicie sessão no [Portal do Azure](<https://portal.azure.com>).

2.  Clique em **adicionar novo recurso**, escreva "maker de qna" em Procurar e selecione o recurso de QnA Maker

    ![Criar um novo serviço de QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Clique em **criar** depois de ler os termos e condições.

    ![Criar um novo serviço de QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. No **Maker de QnA**, selecione as camadas adequadas e regiões.

    ![Criar um novo serviço de QnA Maker](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Preencher o **nome** com um nome exclusivo para identificar este serviço de QnA Maker. Este nome também identifica o ponto final de Maker de QnA ao qual será associado ao seu bases de dados de conhecimento.
    * Escolha o **subscrição** no qual o recurso de QnA Maker será implementado.
    * Selecione o **gestão escalão de preço** para os serviços de gestão de QnA Maker (portal e APIs de gestão). Consulte [aqui](https://aka.ms/qnamaker-pricing) para obter detalhes sobre os preços dos SKUs.
    * Crie um novo **grupo de recursos** (recomendado) ou utilize uma já existente no qual pretende implementar este recurso de QnA Maker.
    * Escolha o **pesquisa escalão de preço** do serviço da Azure Search. Se vir a opção de escalão gratuito esbatida, significa que já tem uma camada de pesquisa do Azure gratuita implementada na sua subscrição. Nesse caso, terá de começar a utilizar o escalão básico da Azure Search. Ver detalhes dos preços da pesquisa do Azure [aqui](https://azure.microsoft.com/en-us/pricing/details/search/).
    * Escolha o **pesquisa localização** onde pretende que os dados de pesquisa do Azure para ser implementada. Restrições na onde os dados de cliente tem de ser armazenados irão informar a localização que escolher para a Azure Search.
    * Forneça um nome para o serviço de aplicações no **nome da aplicação**.
    * Por predefinição, o serviço de aplicações predefinições para o escalão standard do (S1). Pode alterar o plano após a criação. Ver mais detalhes dos preços do App service [aqui](https://azure.microsoft.com/en-in/pricing/details/app-service/).
    * Escolha o **localização do Web site** onde o serviço de aplicação será implementado.

        > [!NOTE]
        > A localização de pesquisa pode ser diferente da localização de Web site.

    * Escolha se pretende ativar **Application Insights** ou não. Se **Application Insights** está ativada, Maker de QnA recolhe telemetria tráfego, erros e registos de chat.
    * Escolha o **localização das informações da aplicação** onde o recurso do Application Insights será implementado.

5. Depois de todos os campos são validados, pode clicar em **criar** para iniciar a implementação destes serviços na sua subscrição. Irá demorar alguns minutos a concluir.

6.  Após a conclusão da implementação, verá os seguintes recursos criados na sua subscrição.

    ![Criar um novo serviço de QnA Maker](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar e publicar uma base de dados de conhecimento](../Quickstarts/create-publish-knowledge-base.md)