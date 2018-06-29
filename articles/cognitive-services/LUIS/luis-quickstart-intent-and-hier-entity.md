---
title: Tutorial para criar uma aplicação LUIS para obter dados de localização - Azure | Microsoft Docs
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e uma entidade hierárquica para extrair dados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: v-geberr
ms.openlocfilehash: 2547407126943161ba604fa2f5e80b9186cae57e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266503"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Tutorial: Criar uma aplicação que utiliza a entidade hierárquica
Neste tutorial, vai criar uma aplicação que demonstra como localizar fragmentos de dados relacionados com base no contexto. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades hierárquicas e os elementos subordinados com contexto adquirido 
> * Criar uma nova aplicação LUIS para um domínio de viagens com a intenção Bookflight
> * Adicionar a intenção _None_ (Nenhuma) e adicionar expressões de exemplo
> * Adicionar a entidade hierárquica de localização com elementos subordinados de origem e de destino
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS incluindo elementos subordinados hierárquicos 

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="purpose-of-the-app-with-this-entity"></a>Objetivo da aplicação com esta entidade
Esta aplicação determina se um utilizador pretende reservar um voo. Utiliza a entidade hierárquica para determinar as localizações, cidade de origem e cidade de destino, no texto do utilizador. 

A entidade hierárquica é uma boa opção para este tipo de dados porque os dois fragmentos de dados:

* São ambos localizações, normalmente expressas como cidades ou códigos de aeroporto.
* Normalmente, têm uma opção de palavras exclusiva em torno das palavras para poder determinar qual é a origem e qual é o destino. Estas palavras incluem: para em direção a, de, partida.
* Ambas as localizações estão frequentemente na mesma expressão. 

O objetivo da entidade **hierárquica** é encontrar dados relacionados na expressão com base no contexto. Considere a seguinte expressão:

```JSON
1 ticket from Seattle to Cairo`
```

A expressão tem duas localizações especificadas. Uma é a cidade de origem, Seattle, e a outro é a cidade de destino, Cairo. Ambas as cidades são importantes para reservar um voo. Embora possam ser encontradas através de entidades simples, estão relacionadas entre si e serão frequentemente encontradas na mesma expressão. Por isso, faz sentido que ambas sejam agrupadas como elementos subordinados de uma entidade hierárquica, **"Location"**. 

Como são entidades de aprendizagem automática, a aplicação precisa de expressões de exemplo com as cidades de origem e de destino identificadas. Isto indica ao LUIS onde as localizações se encontram nas expressões, o comprimento das mesmas e as palavras em torno delas. 

## <a name="app-intents"></a>Intenções da aplicação
As intenções são categorias daquilo que o utilizador pretende. Esta aplicação tem duas intenções: BookFlight e None. A intenção [None](luis-concept-intent.md#none-intent-is-fallback-for-app) é intencional, para indicar qualquer coisa fora da aplicação.  

## <a name="hierarchical-entity-is-contextually-learned"></a>A entidade hierárquica é aprendida contextualmente 
O objetivo da entidade é localizar e categorizar partes do texto na expressão. Uma entidade [hierárquica](luis-concept-entity-types.md) é uma entidade principal-subordinada com base no contexto da utilização. Uma pessoa pode determinar as cidades de origem e de destino numa expressão com base na utilização de `to` e `from`. Estes são um exemplo de utilização contextual.  

Para esta aplicação de Viagens, o LUIS extrai as localizações de origem e de destino de forma a que possa ser criada e preenchida uma reserva padrão. O LUIS permite que as expressões tenham variações, abreviaturas e gíria. 

As expressões de exemplo simples dos utilizadores incluem:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Researve a seat from New York to Paris on the first of April
```

As versões abreviadas ou de gíria das expressões incluem:

```
LHR tomorrow
SEA to NYC next Monday
LA to MCO spring break
```
 
A entidade hierárquica corresponde à localização de origem e de destino. Se estiver presente apenas um elemento subordinado (origem ou destino) de uma entidade hierárquica, ainda assim é extraído. Não é necessário que todos os elementos subordinados sejam encontrados para apenas um ou alguns serem extraídos. 

## <a name="what-luis-does"></a>O que faz o LUIS
Quando a intenção e as entidades da expressão são identificadas, [extraídas](luis-concept-data-extraction.md#list-entity-data) e devolvidas em JSON a partir do [ponto final](https://aka.ms/luis-endpoint-apis), o LUIS está concluído. A aplicação de chamada ou chatbot pega nessa resposta JSON e satisfaz o pedido, da forma que a aplicação ou o chatbot foi concebido para o fazer. 

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS][LUIS]. Certifique-se de que inicia sessão na [região][LUIS-regions] onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS][LUIS], selecione **Create new app** (Criar nova aplicação).  

    [![](media/luis-quickstart-intent-and-hier-entity/app-list.png "Captura de ecrã da página Listas de aplicações")](media/luis-quickstart-intent-and-hier-entity/app-list.png#lightbox)

3. Na caixa de diálogo de pop-up, introduza o nome `MyTravelApp`. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-app.png "Captura de ecrã da caixa de diálogo de pop-up Criar nova aplicação")](media/luis-quickstart-intent-and-hier-entity/create-new-app.png#lightbox)

4. Quando esse processo terminar, a aplicação mostra a página **Intents** (Intenções) com a intenção **None** (Nenhuma). 

    [![](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png "Captura de ecrã da lista de intenções com apenas a intenção None")](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Criar uma nova intenção

1. Na página **Intents** (Intenções), selecione **Create new intent** (Criar nova intenção). 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png "Captura de ecrã da lista de intenções com o botão Criar nova intenção realçado")](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png#lightbox)

2. Introduza o nome da nova intenção `BookFlight`. Esta intenção deve ser selecionada sempre que um utilizador pretenda fazer reservas de voos.

    Ao criar uma intenção, está a criar a categoria principal de informações que pretende identificar. Atribuir um nome à categoria permite que qualquer outra aplicação que utilize os resultados da consulta do LUIS utilize esse nome de categoria para encontrar uma resposta adequada ou para executar a ação apropriada. O LUIS não responde a estas perguntas, apenas identifica o tipo de informação que está a ser pedida em linguagem natural. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png "Captura de ecrã da caixa de diálogo de pop-up Criar nova intenção")](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png#lightbox)

3. Adicione várias expressões à intenção `BookFlight`, que espera que um utilizador peça, tais como:

    | Expressões de exemplo|
    |--|
    |Reservar 2 voos de Seattle para Cairo na próxima segunda-feira|
    |Reservar um bilhete para Londres amanhã|
    |Marcar 4 lugares de Paris para Londres para 1 de abril|

    [![](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png "Captura de ecrã da introdução de expressões na página da intenção BookFlight")](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adicionar expressões à intenção None

Atualmente, a aplicação LUIS não tem expressões para a intenção **None** (Nenhuma). Precisa de expressões que não queira que a aplicação responda e, por isso, tem de ter expressões na intenção **None** (Nenhuma). Não deixe em branco. 

1. Selecione **Intents** (Intenções) no painel esquerdo. 

    [![](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png "Captura de ecrã da página da intenção BookFlight com o botão Intenções realçado")](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png#lightbox)

2. Selecione a intenção **None** (Nenhuma). Adicione três expressões que o utilizador poderá introduzir, mas que não são relevantes para a sua aplicação:

    | Expressões de exemplo|
    |--|
    |Cancelar!|
    |Adeus|
    |O que está a acontecer?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Quando a expressão é prevista para a intenção None
Na sua aplicação de chamada do LUIS (como um chatbot), quando o LUIS devolve a intenção **None** (Nenhuma) para uma expressão, o seu bot pode perguntar se o utilizador quer terminar a conversa. O bot também pode dar mais instruções para continuar a conversa se o utilizador não pretender terminá-la. 

As entidades funcionam na intenção **None** (Nenhuma). Se a intenção com a melhor classificação for **None**, mas for extraída uma entidade que é relevante para o seu chatbot, o chatbot pode dar seguimento com uma pergunta que realce a intenção do cliente. 

## <a name="create-a-location-entity-from-the-intent-page"></a>Criar uma entidade de localização a partir da página Intenção
Agora que as duas intenções têm expressões, o LUIS tem de compreender o que é uma localização. Navegue de volta para a intenção `BookFlight` e identifique (marque) o nome da cidade numa expressão, seguindo os passos:

1. Volte à intenção `BookFlight`, selecionando **Intents** (Intenções) no painel esquerdo.

2. Selecione `BookFlight` na lista de intenções.

3. Na expressão `Book 2 flights from Seattle to Cairo next Monday`, selecione a palavra `Seattle`. É apresentado um menu pendente com uma caixa de texto na parte superior para criar uma nova entidade. Introduza o nome da entidade `Location` na caixa de texto e selecione **Create new entity** (Criar nova entidade) no menu pendente. 

    [![](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png "Captura de ecrã da página da intenção BookFlight, criando uma nova entidade a partir do texto selecionado")](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png#lightbox)

4. Na janela de pop-up, selecione o tipo de entidade **Hierarchical** (Hierárquica) com `Origin` e `Destination` como as entidades subordinadas. Selecione **Done** (Concluído).

    [![](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png "Captura de ecrã da caixa de diálogo de pop-up de criação de entidade para a nova entidade de Localização")](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png#lightbox)

    O identificador para `Seattle` está assinalado como `Location` porque o LUIS não sabe se o termo corresponde à origem ou ao destino, ou a nenhum dos dois. Selecione `Seattle`, selecione Location (Localização), siga o menu à direita e selecione `Origin`.

5. Agora que a entidade está criada e uma expressão está identificada, identifique as outras cidades, selecionando o nome da cidade, Location (Localização) e seguindo o menu à direita para selecionar `Origin` ou `Destination`.

    [![](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png "Captura de ecrã da entidade Bookflight com o texto da expressão selecionado para a seleção da entidade")](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-hier-entity/publish.png "Captura de ecrã da intenção BookFlight com o botão Publicar realçado")](media/luis-quickstart-intent-and-hier-entity/publish.png#lightbox)

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Captura de ecrã da página Publicar com o URL de ponto final realçado")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Vá para o final do URL no endereço e introduza `1 ticket to Portland on Friday`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `BookFlight` com a entidade hierárquica extraída.

```
{
  "query": "1 ticket to Portland on Friday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9998226
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.9998226
    },
    {
      "intent": "None",
      "score": 0.221926212
    }
  ],
  "entities": [
    {
      "entity": "portland",
      "type": "Location::Destination",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.564448953
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas duas intenções e uma entidade hierárquica, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos. 

O chatbot tem agora informações suficientes para determinar a ação principal, `BookFlight`, e as informações de localização encontradas na expressão. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Saiba como adicionar uma entidade de lista](luis-quickstart-intent-and-list-entity.md) 

Adicione a [entidade pré-criada](luis-how-to-add-entities.md#add-prebuilt-entity) **number** para extrair o número. 

Adicione a [entidade pré-criada](luis-how-to-add-entities.md#add-prebuilt-entity) **datetimeV2** para extrair as informações de data.


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
