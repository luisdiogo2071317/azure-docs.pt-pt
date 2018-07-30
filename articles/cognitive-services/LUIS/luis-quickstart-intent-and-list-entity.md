---
title: Tutorial para criar uma aplicação LUIS para obter dados listados de correspondência exata de texto - Azure | Microsoft Docs
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e entidades de lista para extrair dados.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: diberry
ms.openlocfilehash: 4ba2ba5d947a112f780579bf4b31ba38cb26ae03
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222975"
---
# <a name="tutorial-4-add-list-entity"></a>Tutorial: 4. Adicionar entidade de lista
Neste tutorial, vai criar uma aplicação que demonstra como obter dados que correspondem a uma lista predefinida. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades de lista 
> * Criar nova aplicação LUIS para o domínio de Recursos Humanos (RH) com a intenção MoveEmployee
> * Adicionar a entidade de lista para extrair Colaboradores da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidade regex](luis-quickstart-intents-regex-entity.md), [importe](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-regex-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `list`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="purpose-of-the-list-entity"></a>Objetivo da entidade de lista
Esta aplicação prevê expressões sobre como mover um colaborador de um edifício para outro. Esta aplicação utiliza uma entidade de lista para extrair um colaborador. O colaborador pode ser referenciado pelo nome, número de telefone, e-mail ou número de segurança social federal dos E.U.A. 

Uma entidade de lista pode conter muitos itens com sinónimos para cada item. Para uma pequena a média empresa, a entidade de lista é utilizada para extrair as informações do colaborador. 

O nome canónico para cada item é o número de colaborador. Para este domínio, os exemplos dos sinónimos são: 

|Objetivo do sinónimo|Valor do sinónimo|
|--|--|
|Nome|John W. Smith|
|Endereço de e-mail|john.w.smith@mycompany.com|
|Extensão do telefone|x12345|
|Número do telemóvel pessoal|425-555-1212|
|Número de segurança social federal dos E.U.A.|123-45-6789|

Uma entidade de lista é uma boa opção para este tipo de dados quando:

* Os valores dos dados são um conjunto conhecido.
* O conjunto não excede os [limites](luis-boundaries.md) máximos do LUIS para este tipo de entidade.
* O texto na expressão é uma correspondência exata com um sinónimo. 

O LUIS extrai o colaborador de tal forma que pode ser criada uma ordem padrão para mover o colaborador pela aplicação do cliente.
<!--
## Example utterances
Simple example utterances for a `MoveEmployee` inent:

```
move John W. Smith from B-1234 to H-4452
mv john.w.smith@mycompany from office b-1234 to office h-4452

```
-->

## <a name="add-moveemployee-intent"></a>Adicionar intenção MoveEmployee

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png#lightbox)

2. Selecione **Criar nova intenção**. 

    [ ![Captura de ecrã da página Intenções com o botão Criar nova intenção realçado](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png#lightbox)

3. Introduza `MoveEmployee` na caixa de diálogo de pop-up e, em seguida, selecione **Concluído**. 

    ![Captura de ecrã da caixa de diálogo Criar nova de intenção com](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Adicione expressões de exemplo à intenção.

    |Expressões de exemplo|
    |--|
    |mover John W. Smith de B-1234 para H-4452|
    |mv john.w.smith@mycompany.com do escritório b-1234 para o escritório h-4452|
    |trocar x12345 para h-1234 amanhã|
    |colocar 425-555-1212 no HH-2345|
    |mover 123-45-6789 de A-4321 para J-23456|
    |mv Jill Jones de D-2345 para J-23456|
    |trocar jill-jones@mycompany.com para M-12345|
    |x23456 para M-12345|
    |425-555-0000 para h-4452|
    |234-56-7891 para hh-2345|

    [ ![Captura de ecrã da página Intenção com as novas expressões realçadas](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

## <a name="create-an-employee-list-entity"></a>Criar uma entidade de lista de colaborador
Agora que a intenção **MoveEmployee** tem expressões, o LUIS tem de compreender o que é um colaborador. 

1. Selecione **Entidades** no painel esquerdo.

    [ ![Captura de ecrã da página Intenção com o botão Entidades no painel de navegação esquerdo destacado](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png)](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png#lightbox)

2. Selecione **Criar nova entidade**.

    [ ![Captura de ecrã da página Entidades com Criar nova entidade realçada](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png#lightbox)

3. Na caixa de diálogo pop-up de entidade, introduza `Employee` para o nome da entidade e **Lista** para o tipo de entidade. Selecione **Done** (Concluído).  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Captura de ecrã da caixa de diálogo criação da nova entidade")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Na página de entidade de colaborador, introduza `Employee-24612` como o novo valor.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Captura de ecrã do valor inserido")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Para Sinónimos, adicione os seguintes valores:

    |Objetivo do sinónimo|Valor do sinónimo|
    |--|--|
    |Nome|John W. Smith|
    |Endereço de e-mail|john.w.smith@mycompany.com|
    |Extensão do telefone|x12345|
    |Número do telemóvel pessoal|425-555-1212|
    |Número de segurança social federal dos E.U.A.|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Captura de ecrã dos sinónimos inseridos")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Introduza o `Employee-45612` como um novo valor.

7. Para Sinónimos, adicione os seguintes valores:

    |Objetivo do sinónimo|Valor do sinónimo|
    |--|--|
    |Nome|Jill Jones|
    |Endereço de e-mail|jill-jones@mycompany.com|
    |Extensão do telefone|x23456|
    |Número do telemóvel pessoal|425-555-0000|
    |Número de segurança social federal dos E.U.A.|234-56-7891|

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Captura de ecrã da seleção do botão Publicar")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Captura de ecrã da seleção do botão Publicar no bloco de produção")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Captura de ecrã do URL do ponto final na página Publicar")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Vá para o final do URL no endereço e introduza `shift 123-45-6789 from Z-1242 to T-54672`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `MoveEmployee` com o `Employee` extraído.

```JSON
{
  "query": "shift 123-45-6789 from Z-1242 to T-54672",
  "topScoringIntent": {
    "intent": "MoveEmployee",
    "score": 0.9882801
  },
  "intents": [
    {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    {
      "intent": "FindForm",
      "score": 0.016044287
    },
    {
      "intent": "GetJobInformation",
      "score": 0.007611245
    },
    {
      "intent": "ApplyForJob",
      "score": 0.007063288
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00684710965
    },
    {
      "intent": "None",
      "score": 0.00304174074
    },
    {
      "intent": "Utilities.Help",
      "score": 0.002981
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00212222221
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00191026414
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0007461446
    }
  ],
  "entities": [
    {
      "entity": "123 - 45 - 6789",
      "type": "Employee",
      "startIndex": 6,
      "endIndex": 16,
      "resolution": {
        "values": [
          "Employee-24612"
        ]
      }
    },
    {
      "entity": "123",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 8,
      "resolution": {
        "value": "123"
      }
    },
    {
      "entity": "45",
      "type": "builtin.number",
      "startIndex": 10,
      "endIndex": 11,
      "resolution": {
        "value": "45"
      }
    },
    {
      "entity": "6789",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 16,
      "resolution": {
        "value": "6789"
      }
    },
    {
      "entity": "-1242",
      "type": "builtin.number",
      "startIndex": 24,
      "endIndex": 28,
      "resolution": {
        "value": "-1242"
      }
    },
    {
      "entity": "-54672",
      "type": "builtin.number",
      "startIndex": 34,
      "endIndex": 39,
      "resolution": {
        "value": "-54672"
      }
    }
  ]
}
```

O colaborador foi encontrado e devolvido como o tipo `Employee` com um valor de resolução de `Employee-24612`.

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Onde está o processamento da linguagem natural na entidade de lista? 
Uma vez que a entidade de lista é uma correspondência exata de texto, não depende do processamento de linguagem natural (ou aprendizagem automática). O LUIS utiliza o processamento de linguagem natural (ou aprendizagem automática) para selecionar a intenção correta com a melhor classificação. Além disso, uma expressão pode ser uma mistura de mais de uma entidade ou mesmo de mais do que um tipo de entidade. Cada expressão é processada para todas as entidades na aplicação, incluindo as entidades de processamento de linguagem natural (ou aprendizagem automática).

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com uma entidade de lista, extraiu o colaborador correto. 

O chatbot tem agora informações suficientes para determinar a ação principal, `MoveEmployee`, e qual colaborador deve mover. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **As minhas aplicações** no menu do canto superior esquerdo. Selecione as reticências (***…***) à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicione uma entidade hierárquica à aplicação](luis-quickstart-intent-and-hier-entity.md)

