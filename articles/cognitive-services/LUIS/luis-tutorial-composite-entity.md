---
title: Tutorial de criação de uma entidade composta para extrair dados complexos - Azure | Documentos da Microsoft
description: Saiba como criar uma entidade composta na sua aplicação LUIS para extrair os diferentes tipos de dados de entidade.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 07/09/2018
ms.author: v-geberr
ms.openlocfilehash: d73dc9b9f204e334a75c9de5e19c6b11e3a95b12
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929190"
---
# <a name="tutorial-6-add-composite-entity"></a>Tutorial: 6. Adicionar entidade composta 
Neste tutorial, adicione uma entidade composta para reunir dados extraídos para uma entidade contentora.

Neste tutorial, ficará a saber como:

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades compostas 
> * Adicionar entidade composta para extrair dados
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidade hierárquica](luis-quickstart-intent-and-hier-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `composite`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original.  

## <a name="composite-entity-is-a-logical-grouping"></a>Entidade composta é um agrupamento lógico 
O objetivo da entidade composto consiste em Agrupar entidades relacionadas numa entidade da categoria principal. As informações existem como entidades separadas antes da criação de uma composição. Ele é semelhante à entidade hierárquica, mas pode conter mais tipos de entidades. 

 Crie uma entidade composta quando as entidades separadas podem ser agrupadas logicamente e este agrupamento lógico é útil para a aplicação cliente. 

Nesta aplicação, o nome do funcionário é definido no **funcionário** listar entidade e inclui sinónimos de nome, endereço de e-mail, a extensão de telefone da empresa, número de telemóvel e dos EUA ID do imposto sobre Federal. 

O **MoveEmployee** intenção tem expressões de exemplo para pedir um funcionário ser movido de um edifício e do office para outro. Os nomes de construção são alfabéticos: "A", "B", etc. Embora escritórios sejam numéricos: "1234", "13245". 

Expressões com de exemplo no **MoveEmployee** intenção incluem:

|Expressões de exemplo|
|--|
|Mova John W. Cardoso a-2345|
|trocar x12345 para h-1234 amanhã|
 
O pedido de movimentação deve incluir, pelo menos, o funcionário (usando qualquer sinónimos) e a localização final de criação e o office. O pedido também pode incluir o escritório de origem, bem como uma data que a movimentação deve acontecer. 

Os dados extraídos do ponto de extremidade devem conter essas informações e retorná-lo num `RequestEmployeeMove` entidade composta. 

## <a name="create-composite-entity"></a>Criar entidade composta
1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-tutorial-composite-entity/hr-first-image.png)](./media/luis-tutorial-composite-entity/hr-first-image.png#lightbox)

2. Sobre o **intenções** página, selecione **MoveEmployee** intenção. 

    [![](media/luis-tutorial-composite-entity/hr-intents-moveemployee.png "Captura de ecrã do LUIS, com a intenção de 'MoveEmployee' realçada")](media/luis-tutorial-composite-entity/hr-intents-moveemployee.png#lightbox)

3. Selecione o ícone de lupa na barra de ferramentas para filtrar a lista de expressões. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Captura de ecrã do LUIS na intenção de 'MoveEmployee' com o botão da Lupa realçado")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Introduza `tomorrow` na caixa de texto filtro para encontrar a expressão `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Captura de ecrã do LUIS na intenção de 'MoveEmployee' com o filtro do 'futuro' realçado")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Outro método é filtrar a entidade por datetimeV2, selecionando **filtros de entidade** , em seguida, selecione **datetimeV2** da lista. 

5. Selecione a primeira entidade `Employee`, em seguida, selecione **encapsular na entidade composta** na lista de menu de pop-up. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Captura de ecrã do LUIS na intenção 'MoveEmployee', selecionar a primeira entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Em seguida, selecione imediatamente a última entidade `datetimeV2` na expressão. Uma barra de verde é desenhada sob as palavras selecionadas que indica uma entidade composta. No menu de pop-up, introduza o nome composto `RequestEmployeeMove` , em seguida, selecione **compostos nova de criar** no menu de pop-up. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Captura de ecrã do LUIS na intenção \"MoveEmployee\" selecionando a última entidade no compostos e criar entidades realçado")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. Na **o tipo de entidade que pretende criar?**, quase todos os campos necessários estão na lista. Apenas a localização de origem está em falta. Selecione **adicionar entidade subordinada**, selecione **Locations::Origin** da lista de entidades existentes, em seguida, selecione **feito**. 

  ![Captura de ecrã do LUIS na intenção "MoveEmployee" adicionar outra entidade na janela de pop-up](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Selecione a Lupa na barra de ferramentas para remover o filtro. 

## <a name="label-example-utterances-with-composite-entity"></a>Expressões com de exemplo de etiqueta com a entidade composta
1. Em cada ocorrência de pronunciação de exemplo, selecione a entidade de mais à esquerda que deve estar na composição. Em seguida, selecione **encapsular na entidade composta**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Captura de ecrã do LUIS na intenção 'MoveEmployee', selecionar a primeira entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Selecione a última palavra na entidade composta, em seguida, selecione **RequestEmployeeMove** no menu de pop-up. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Captura de ecrã do LUIS na intenção \"MoveEmployee\" selecionando a última entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Certifique-se de que todas as expressões na intenção são rotuladas com a entidade composta. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Captura de ecrã do LUIS no MoveEmployee com todas as expressões com o nome")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
LUIS não sabe sobre a nova entidade composta até que a aplicação é preparada. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-tutorial-composite-entity/hr-train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-tutorial-composite-entity/hr-trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    ![publicar aplicação](./media/luis-tutorial-composite-entity/hr-publish-to-production.png)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint"></a>Consultar o ponto final 
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Selecione o URL de ponto final](./media/luis-tutorial-composite-entity/hr-publish-select-endpoint.png)

2. Vá para o final do URL no endereço e introduza `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.`. O último parâmetro de cadeia de consulta é `q`, a consulta de expressão. 

    Uma vez que este teste é verificar que a composição é extraída corretamente, um teste ou pode incluir uma expressão de exemplo existente ou uma expressão de novo. É um bom teste incluir todas as entidades de subordinado na entidade composta.

```JSON
{
  "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
  "topScoringIntent": {
    "intent": "MoveEmployee",
    "score": 0.9959525
  },
  "intents": [
    {
      "intent": "MoveEmployee",
      "score": 0.9959525
    },
    {
      "intent": "GetJobInformation",
      "score": 0.009858314
    },
    {
      "intent": "ApplyForJob",
      "score": 0.00728598563
    },
    {
      "intent": "FindForm",
      "score": 0.0058053555
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.005371796
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00266987388
    },
    {
      "intent": "None",
      "score": 0.00123299169
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00116407464
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00102653319
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0006628214
    }
  ],
  "entities": [
    {
      "entity": "march 3 2 p.m",
      "type": "builtin.datetimeV2.datetime",
      "startIndex": 41,
      "endIndex": 54,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-03-03T14",
            "type": "datetime",
            "value": "2018-03-03 14:00:00"
          },
          {
            "timex": "XXXX-03-03T14",
            "type": "datetime",
            "value": "2019-03-03 14:00:00"
          }
        ]
      }
    },
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 14,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "z - 2345",
      "type": "Locations::Destination",
      "startIndex": 31,
      "endIndex": 36,
      "score": 0.9690751
    },
    {
      "entity": "a - 1234",
      "type": "Locations::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.9713137
    },
    {
      "entity": "-1234",
      "type": "builtin.number",
      "startIndex": 22,
      "endIndex": 26,
      "resolution": {
        "value": "-1234"
      }
    },
    {
      "entity": "-2345",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 36,
      "resolution": {
        "value": "-2345"
      }
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 47,
      "endIndex": 47,
      "resolution": {
        "value": "3"
      }
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 50,
      "endIndex": 50,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
      "type": "requestemployeemove",
      "startIndex": 5,
      "endIndex": 54,
      "score": 0.4027723
    }
  ],
  "compositeEntities": [
    {
      "parentType": "requestemployeemove",
      "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
      "children": [
        {
          "type": "builtin.datetimeV2.datetime",
          "value": "march 3 2 p.m"
        },
        {
          "type": "Locations::Destination",
          "value": "z - 2345"
        },
        {
          "type": "Employee",
          "value": "jill jones"
        },
        {
          "type": "Locations::Origin",
          "value": "a - 1234"
        }
      ]
    }
  ],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

Esta expressão devolve uma matriz de entidades compostos. Cada entidade é dado um tipo e valor. Para localizar mais de precisão de cada entidade subordinada, utilize a combinação de tipo e o valor do item de matriz compostos para localizar o item correspondente da matriz de entidades.  

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação identificada uma intenção da consulta de linguagem natural e devolveu os dados extraídos como um grupo nomeado. 

Agora, seu chatbot tem informações suficientes para determinar a ação principal e os detalhes relacionados na expressão. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **as minhas aplicações** no menu à esquerda superior. Selecione as reticências (***...*** ) botão à direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"] 
> [Saiba como adicionar uma entidade com uma lista de frase](luis-quickstart-primary-and-secondary-data.md)  