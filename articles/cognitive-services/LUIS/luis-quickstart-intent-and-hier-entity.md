---
title: Tutorial para criar uma aplicação LUIS para obter dados de localização - Azure | Microsoft Docs
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e uma entidade hierárquica para extrair dados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/22/2018
ms.author: v-geberr
ms.openlocfilehash: 5fb93ebbd2da02df0c2cdf0d19ed282aeafe9473
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335565"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Tutorial: Criar uma aplicação que utiliza a entidade hierárquica
Neste tutorial, vai criar uma aplicação que demonstra como localizar fragmentos de dados relacionados com base no contexto. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades hierárquicas e os elementos subordinados com contexto adquirido 
> * Utilizar a aplicação LUIS no domínio de Recursos Humanos (RH) 
> * Adicionar a entidade hierárquica de localização com elementos subordinados de origem e de destino
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS incluindo elementos subordinados hierárquicos 

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidades de lista](luis-quickstart-intent-and-list-entity.md), [importe](create-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-list-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `hier`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="purpose-of-the-app-with-this-entity"></a>Objetivo da aplicação com esta entidade
Esta aplicação determina para onde um funcionário deve ser deslocado da localização de origem (edifício e escritório) para a localização de destino (edifício e escritório). Utiliza a entidade hierárquica para determinar as localizações na expressão. 

A entidade hierárquica é uma boa opção para este tipo de dados porque os dois fragmentos de dados:

* Estão relacionados entre si no contexto da expressão.
* Utilize a opção de palavras específica para indicar cada localização. Exemplos destas palavras: de/para, deixar/em direção a, sair de/para.
* Ambas as localizações estão frequentemente na mesma expressão. 

O objetivo da entidade **hierárquica** é encontrar dados relacionados na expressão com base no contexto. Considere a seguinte expressão:

```JSON
mv Jill Jones from a-2349 to b-1298
```
A expressão tem duas localizações especificadas, `a-2349` e `b-1298`. Assuma que a letra corresponde a um nome de edifício e o número indica o escritório nesse edifício. Faz sentido que estejam ambos agrupados como subordinados de uma entidade hierárquica, `Locations`, porque ambos os fragmentos de dados têm de ser extraídos da expressão e estão relacionados entre si. 
 
Se estiver presente apenas um elemento subordinado (origem ou destino) de uma entidade hierárquica, ainda assim é extraído. Não é necessário que todos os elementos subordinados sejam encontrados para apenas um ou alguns serem extraídos. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Remover a entidade de número pré-concebida da aplicação
Para ver toda a expressão e marcar os subordinados hierárquicos, remova temporariamente a entidade de número pré-concebida.

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-hier-entity/hr-first-image.png#lightbox)

2. Selecione **Entidades** no menu esquerdo.

    [ ![Captura de ecrã da aplicação LUIS com o botão Entidades realçado no menu esquerdo](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entities-button.png#lightbox)


3. Selecione as reticências (…) à direita da entidade de número na lista. Selecione **Eliminar**. 

    [ ![Captura de ecrã da aplicação LUIS na página Lista de entidades, com o botão Eliminar realçado para a entidade de número pré-concebida](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png)](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png#lightbox)


## <a name="add-utterances-to-findform-intent"></a>Adicionar expressões à intenção FindForm

1. Selecione **Intents** (Intenções) no menu da esquerda.

    [ ![Captura de ecrã da aplicação LUIS com Intenções realçado no menu esquerdo](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-intents-button.png#lightbox)

2. Selecione **MoveEmployee** da lista de intenções.

    [ ![Captura de ecrã da aplicação LUIS com a intenção MoveEmployee realçada no menu esquerdo](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png)](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png#lightbox)

3. Adicione as seguintes expressões de exemplo:

    |Expressões de exemplo|
    |--|
    |Mover John W. Smith **para** a-2345|
    |Direcionar Jill Jones **para** b-3499|
    |Organizar a mudança de x23456 **de** hh-2345 **para** e-0234|
    |Iniciar a burocracia para definir x12345 para **deixar** a-3459 **em direção a** f-34567|
    |Deslocar 425-555-0000 para **sair de** g-2323 **para** hh-2345|

    No tutorial [entidade de lista](luis-quickstart-intent-and-list-entity.md), um funcionário pode ser designado por nome, endereço de e-mail, extensão do telefone, número de telemóvel ou número da segurança social. Estes números de funcionários são utilizados nas expressões. As expressões de exemplo anteriores incluem diferentes formas para anotar as localizações de origem e destino, marcadas a negrito. Algumas das expressões apenas têm destinos de propósito. Isto ajuda o LUIS a compreender como essas localizações são colocadas na expressão quando a origem não está especificada.

    [ ![Captura de ecrã do LUIS com as novas expressões na intenção MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)
     

## <a name="create-a-location-entity"></a>Criar uma entidade de localização
O LUIS tem de compreender o que é uma localização ao etiquetar a origem e o destino nas expressões. Se precisar de ver a expressão na vista de tokens (não processados), selecione o botão de alternar na barra acima das expressões etiquetadas como **Vista de Entidades**. Depois de alternar o comutador, o controlo está etiquetado como **Vista de Tokens**.

1. Na expressão `Displace 425-555-0000 away from g-2323 toward hh-2345`, selecione a palavra `g-2323`. É apresentado um menu pendente com uma caixa de texto na parte superior. Introduza o nome da entidade `Locations` na caixa de texto e selecione **Create new entity** (Criar nova entidade) no menu pendente. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png "Captura de ecrã da criação da nova entidade na página de intenções")](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png#lightbox)

2. Na janela de pop-up, selecione o tipo de entidade **Hierarchical** (Hierárquica) com `Origin` e `Destination` como as entidades subordinadas. Selecione **Done** (Concluído).

    ![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Captura de ecrã da caixa de diálogo de pop-up de criação de entidade para a nova entidade de Localização")

3. O identificador para `g-2323` está assinalado como `Locations` porque o LUIS não sabe se o termo corresponde à origem ou ao destino, ou a nenhum dos dois. Selecione `g-2323`, selecione **Locations** e, em seguida, siga o menu à direita e selecione `Origin`.

    [![](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png "Captura de ecrã da caixa de diálogo de pop-up de etiquetagem da entidade para alterar o subordinado da entidade Localizações")](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png#lightbox)

5. Etiquete as outras localizações em todas as expressões ao selecionar o edifício e o escritório na expressão, selecione Localizações e siga o menu à direita para selecionar `Origin` ou `Destination`. Quando todas as localizações estiverem etiquetadas, as expressões na **Vista de Tokens** começa a parecer um padrão. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png "Captura de ecrã da entidade Localizações etiquetada nas expressões")](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png#lightbox)

## <a name="add-prebuilt-number-entity-to-app"></a>Adicionar a entidade de número pré-concebida à aplicação
Adicione a entidade de número pré-concebida novamente à aplicação.

1. Selecione **Entidades** no menu de navegação esquerdo.

    [ ![Captura de ecrã do botão Entidades realçado no painel de navegação esquerdo](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png)](./media/luis-quickstart-intent-and-hier-entity/hr-select-entity-button-from-intent-page.png#lightbox)

2. Selecione o botão **Gerir entidades pré-concebidas**.

    [ ![Captura de ecrã da Lista de entidades com Gerir entidades pré-concebidas realçado](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png#lightbox)

3. Selecione **número** na lista de entidades pré-concebidas e, em seguida, selecione **Concluído**.

    ![Captura de ecrã do número selecionado na caixa de diálogo de entidades pré-concebidas](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Captura de ecrã da página Publicar com o URL de ponto final realçado")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Vá para o final do URL na barra de endereço e introduza `Please relocation jill-jones@mycompany.com from x-2345 to g-23456`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `MoveEmployee` com a entidade hierárquica extraída.

```JSON
{
  "query": "Please relocation jill-jones@mycompany.com from x-2345 to g-23456",
  "topScoringIntent": {
    "intent": "MoveEmployee",
    "score": 0.9966052
  },
  "intents": [
    {
      "intent": "MoveEmployee",
      "score": 0.9966052
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0325253047
    },
    {
      "intent": "FindForm",
      "score": 0.006137873
    },
    {
      "intent": "GetJobInformation",
      "score": 0.00462633232
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00415637763
    },
    {
      "intent": "ApplyForJob",
      "score": 0.00382325822
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00249120337
    },
    {
      "intent": "None",
      "score": 0.00130756292
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00119622645
    },
    {
      "intent": "Utilities.Confirm",
      "score": 1.26910036E-05
    }
  ],
  "entities": [
    {
      "entity": "jill - jones @ mycompany . com",
      "type": "Employee",
      "startIndex": 18,
      "endIndex": 41,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "x - 2345",
      "type": "Locations::Origin",
      "startIndex": 48,
      "endIndex": 53,
      "score": 0.8520272
    },
    {
      "entity": "g - 23456",
      "type": "Locations::Destination",
      "startIndex": 58,
      "endIndex": 64,
      "score": 0.974032
    },
    {
      "entity": "-2345",
      "type": "builtin.number",
      "startIndex": 49,
      "endIndex": 53,
      "resolution": {
        "value": "-2345"
      }
    },
    {
      "entity": "-23456",
      "type": "builtin.number",
      "startIndex": 59,
      "endIndex": 64,
      "resolution": {
        "value": "-23456"
      }
    }
  ]
}
```

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Podia ter utilizado uma expressão regular para cada localização?
Sim. Pode criar a expressão regular com as funções de origem e destino e utilizá-la num padrão.

As localizações neste exemplo, como `a-1234`, seguem um formato específico de uma ou duas letras com um travessão e uma série de 4 ou 5 números. Estes dados podem ser descritos como uma entidade de expressão regular com uma função para cada localização. As funções estão disponíveis para os padrões. Pode criar padrões com base nestas expressões e, em seguida, criar uma expressão regular para o formato de localização e adicioná-la aos padrões. <!-- Go to this tutorial to see how that is done -->

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas algumas intenções e uma entidade hierárquica, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos. 

O chatbot tem agora informações suficientes para determinar a ação principal, `MoveEmployee`, e as informações de localização encontradas na expressão. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Saiba como adicionar uma entidade de lista](luis-quickstart-intent-and-list-entity.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
