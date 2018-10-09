---
title: 'Tutorial 5: relações principais/subordinadas – entidade hierárquica do LUIS para dados adquiridos contextualmente'
titleSuffix: Azure Cognitive Services
description: Localize fragmentos de dados relacionados com base no contexto. Por exemplo, as localizações de origem e destino para uma mudança física de um edifício e escritório para outros estão relacionadas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 92b6327cbb97ed871cd4b10977bcd73a81494e20
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042130"
---
# <a name="tutorial-5-extract-contextually-related-data"></a>Tutorial 5: extrair dados relacionados contextualmente
Neste tutorial, localize fragmentos de dados relacionados com base no contexto. Por exemplo, as localizações de origem e destino para uma mudança física de um edifício e escritório para outros estão relacionadas. Para gerar uma ordem de trabalho, ambos os fragmentos de dados podem ser necessários e estão relacionados entre si.  

Esta aplicação determina para onde um funcionário deve ser deslocado da localização de origem (edifício e escritório) para a localização de destino (edifício e escritório). Utiliza a entidade hierárquica para determinar as localizações na expressão. O objetivo da entidade **hierárquica** é encontrar dados relacionados na expressão com base no contexto. 

A entidade hierárquica é uma boa opção para este tipo de dados porque os dois fragmentos de dados:

* São entidades simples.
* Estão relacionados entre si no contexto da expressão.
* Utilize a opção de palavras específica para indicar cada localização. Exemplos destas palavras: de/para, deixar/em direção a, sair de/para.
* Ambas as localizações estão frequentemente na mesma expressão. 
* Têm de ser agrupadas e processadas pela aplicação cliente como uma unidade de informações.

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar intenções 
> * Adicionar a entidade hierárquica de localização com elementos subordinados de origem e de destino
> * Preparar
> * Publicar
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-list-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `hier`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Remover a entidade de número pré-concebida da aplicação
Para ver toda a expressão e marcar os subordinados hierárquicos, remova temporariamente a entidade de número pré-concebida.

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Entidades** no menu esquerdo.

3. Selecione o botão de reticências (***…***) à direita da entidade de número na lista. Selecione **Eliminar**. 

## <a name="add-utterances-to-moveemployee-intent"></a>Adicionar expressões à intenção MoveEmployee

1. Selecione **Intents** (Intenções) no menu esquerdo.

2. Selecione **MoveEmployee** da lista de intenções.

3. Adicione as seguintes expressões de exemplo:

    |Expressões de exemplo|
    |--|
    |Mover John W. Smith **para** a-2345|
    |Direcionar Jill Jones **para** b-3499|
    |Organizar a mudança de x23456 **de** hh-2345 **para** e-0234|
    |Iniciar a burocracia para definir x12345 para **deixar** a-3459 **em direção a** f-34567|
    |Deslocar 425-555-0000 para **sair de** g-2323 **para** hh-2345|

    [ ![Captura de ecrã do LUIS com as novas expressões na intenção MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    No tutorial [entidade de lista](luis-quickstart-intent-and-list-entity.md), um colaborador é designado por nome, endereço de e-mail, extensão do telefone, número de telemóvel ou número da segurança social. Estes números de funcionários são utilizados nas expressões. As expressões de exemplo anteriores incluem diferentes formas para anotar as localizações de origem e destino, marcadas a negrito. Algumas das expressões apenas têm destinos de propósito. Isto ajuda o LUIS a compreender como essas localizações são colocadas na expressão quando a origem não está especificada.     

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="create-a-location-entity"></a>Criar uma entidade de localização
O LUIS tem de compreender o que é uma localização ao etiquetar a origem e o destino nas expressões. Se precisar de ver a expressão na vista de tokens (não processados), selecione o botão de alternar na barra acima das expressões etiquetadas como **Vista de Entidades**. Depois de alternar o comutador, o controlo está etiquetado como **Vista de Tokens**.

Considere a seguinte expressão:

```JSON
mv Jill Jones from a-2349 to b-1298
```

A expressão tem duas localizações especificadas, `a-2349` e `b-1298`. Assuma que a letra corresponde a um nome de edifício e o número indica o escritório nesse edifício. Faz sentido que estejam ambos agrupados como subordinados de uma entidade hierárquica, `Locations`, porque ambos os fragmentos de dados têm de ser extraídos da expressão para concluir o pedido na aplicação cliente e porque estão relacionados entre si. 
 
Se estiver presente apenas um elemento subordinado (origem ou destino) de uma entidade hierárquica, ainda assim é extraído. Não é necessário que todos os elementos subordinados sejam encontrados para apenas um ou alguns serem extraídos. 

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

2. Selecione o botão **Gerir entidades pré-concebidas**.

3. Selecione **número** na lista de entidades pré-concebidas e, em seguida, selecione **Concluído**.

    ![Captura de ecrã do número selecionado na caixa de diálogo de entidades pré-concebidas](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


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
    
    A intenção correta é prevista e a matriz de entidades tem os valores de origem e destino na propriedade **entity** correspondente.
    

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Podia ter utilizado uma expressão regular para cada localização?
Sim. Pode criar a entidade de expressão regular com as funções de origem e destino e utilizá-la num padrão.

As localizações neste exemplo, como `a-1234`, seguem um formato específico de uma ou duas letras com um travessão e uma série de 4 ou 5 números. Estes dados podem ser descritos como uma entidade de expressão regular com uma função para cada localização. As funções só estão disponíveis para padrões. Pode criar padrões com base nestas expressões e, em seguida, criar uma expressão regular para o formato de localização e adicioná-la aos padrões. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, criou uma nova intenção e adicionou expressões de exemplo para dados adquiridos contextualmente das localizações de origem e destino. Depois de a aplicação ser preparada e publicada, uma aplicação cliente pode utilizar essas informações para criar um pedido de movimentação com as informações relevantes.

> [!div class="nextstepaction"] 
> [Saiba como adicionar uma entidade composta](luis-tutorial-composite-entity.md) 