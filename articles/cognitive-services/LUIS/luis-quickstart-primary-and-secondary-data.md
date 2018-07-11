---
title: 'Tutorial: criar uma aplicação LUIS para extrair dados - Azure | Microsoft Docs'
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e uma entidade simples para extrair dados de aprendizagem automática.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: e6ab9d1db0144ffa68fe9dc3381ba31d57aa0cae
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130897"
---
# <a name="tutorial-6-add-simple-entity-and-phrase-list"></a>Tutorial: 6. Adicionar entidade simples e lista de expressões
Neste tutorial, vai criar uma aplicação que demonstra como extrair dados de aprendizagem automática de uma expressão com a entidade **Simple** (Simples).

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades simples 
> * Criar uma nova aplicação LUIS para o domínio de Recursos Humanos (RH) 
> * Adicionar uma entidade simples para extrair tarefas da aplicação
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS
> * Adicionar a lista de expressões para melhorar o sinal das palavras da tarefa
> * Preparar, publicar a aplicação e repetir a consulta do ponto final

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidade hierárquica](luis-quickstart-intent-and-hier-entity.md), [importe](create-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `simple`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original.  

## <a name="purpose-of-the-app"></a>Objetivo da aplicação
Esta aplicação demonstra como extrair dados de uma expressão. Considere as seguintes expressões de um chatbot:

|Expressão|Nome da tarefa passível de extração|
|:--|:--|
|Quero candidatar-me ao novo trabalho de contabilidade.|contabilidade|
|Submeter o meu currículo para o novo cargo de engenharia.|engenharia|
|Preencher a candidatura para o trabalho 123456|123456|

Este tutorial adiciona uma nova entidade para extrair o nome do trabalho. 

## <a name="purpose-of-the-simple-entity"></a>Objetivo da entidade simples
O objetivo da entidade simples nesta aplicação LUIS é ensinar ao LUIS o que é um nome de trabalho e onde pode ser encontrado numa expressão. A parte da expressão que corresponde ao trabalho pode variar de expressão para expressão com base na escolha de palavras e no comprimento da expressão. O LUIS precisa de exemplos de trabalhos em qualquer expressão em todas as intenções.  

O nome do trabalho é difícil de determinar porque um nome pode ser um substantivo, verbo ou uma expressão de várias palavras. Por exemplo:

|Tarefas|
|--|
|engenheiro|
|engenheiro de software|
|engenheiro de software sénior|
|chefe da equipa de engenharia |
|controlador de tráfego aéreo|
|operador de veículos motorizados|
|motorista de ambulância|
|encarregado|
|operador de extrusora|
|mecânico afinador de máquinas|

Esta aplicação LUIS tem nomes de trabalhos em várias intenções. Ao identificar estas palavras nas expressões de todas as intenções, o LUIS aprende mais sobre um trabalho e onde pode ser encontrado nas expressões.

## <a name="create-job-simple-entity"></a>Criar uma entidade simples do trabalho

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png)](./media/luis-quickstart-primary-and-secondary-data/hr-first-image.png#lightbox)

2. Na página **Intents** (Intenções), selecione a intenção **ApplyForJob**. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png "Captura de ecrã do LUIS com a intenção «ApplyForJob» realçada")](media/luis-quickstart-primary-and-secondary-data/hr-select-applyforjob.png#lightbox)

3. Na expressão, `I want to apply for the new accounting job`, selecione `accounting`, introduza `Job` no campo superior do menu de pop-up e, em seguida, selecione **Create new entity** (Criar nova entidade) no menu de pop-up. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png "Captura de ecrã do LUIS com a intenção \"ApplyForJob\" com os passos de criação de entidade realçados")](media/luis-quickstart-primary-and-secondary-data/hr-create-entity.png#lightbox)

4. Na janela de pop-up, verifique o nome e o tipo da entidade e selecione **Done** (Concluído).

    ![Criar um diálogo modal de pop-up de entidade simples com o nome do Trabalho e o tipo simples](media/luis-quickstart-primary-and-secondary-data/hr-create-simple-entity-popup.png)

5. Na expressão, `Submit resume for engineering position`, identifique a palavra `engineering` como a entidade de Trabalho. Selecione a palavra `engineering` e, em seguida, selecione **Trabalho** no menu de pop-up. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png "Captura de ecrã do LUIS com a identificação da entidade Job realçada")](media/luis-quickstart-primary-and-secondary-data/hr-label-simple-entity.png#lightbox)

    Todas as expressões estão identificadas, mas cinco expressões não são suficientes para ensinar ao LUIS sobre palavras e expressões relacionadas com trabalho. Os trabalhos que utilizam o valor numérico não precisam de mais exemplos porque utilizam uma entidade de expressão regular. Os trabalhos que são palavras ou expressões precisam de, pelo menos, mais 15 exemplos. 

6. Adicione mais expressões e marque as palavras ou expressões de trabalho como entidade **Job** (Trabalho). Os tipos de trabalho são gerais em todos os empregos para um serviço de emprego. Se quisesse trabalhos relacionados com um setor específico, as palavras do trabalho devem refletir esse setor. 

    |Expressão|Entidade de trabalho|
    |:--|:--|
    |Estou a candidatar-me à receção de Gestor de Programas em I&D|Gestor de Programas|
    |Segue a minha candidatura a cozinheiro de linha.|cozinheiro de linha|
    |Envio em anexo o meu currículo para a vaga de conselheiro de acampamento.|conselheiro de acampamento|
    |Este é o meu CV. para assistente administrativo.|para assistente administrativo|
    |Quero candidatar-me à oferta de emprego de gestão em vendas.|gestão, vendas|
    |Este é o meu currículo para o novo cargo de contabilidade.|contabilidade|
    |Incluo a minha candidatura para barman.|barman|
    |Estou a enviar a minha candidatura para reparador de telhados e construtor civil.|reparador de telhados, construtor civil|
    |O meu CV para motorista de autocarro está aqui.|motorista de autocarro|
    |Sou enfermeira licenciada. Aqui está o meu currículo.|enfermeira licenciada|
    |Gostaria de submeter a minha documentação para o cargo de professor que vi no jornal.|professor|
    |Este é o meu CV. para a vaga de repositor de frutas e vegetais.|repositor|
    |Candidatar a trabalho de aplicação de mosaicos.|mosaicos|
    |Currículo anexado para arquiteto paisagista.|arquiteto paisagista|
    |Envio em anexo o meu curriculum vitae para professor de biologia.|professor de biologia|
    |Gostaria de candidatar-me ao cargo na área de fotografia.|fotografia|git 

## <a name="label-entity-in-example-utterances-for-getjobinformation-intent"></a>Identificar a entidade nas expressões de exemplo para a intenção GetJobInformation
1. Selecione **Intents** (Intenções) no menu esquerdo.

2. Selecione **GetJobInformation** na lista de intenções. 

3. Identifique os trabalhos nas expressões de exemplo:

    |Expressão|Entidade de trabalho|
    |:--|:--|
    |Existe algum trabalho em bases de dados?|bases de dados|
    |Procuro um novo emprego com responsabilidades em contabilidade|contabilidade|
    |Que vagas estão disponíveis para engenheiros sénior?|engenheiros sénior|

    Existem outras expressões de exemplo, mas não contêm palavras relacionadas com trabalhos.

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Selecionar o botão de preparação](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Notificação de êxito de preparação](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Captura de ecrã da página Publicar com o ponto final realçado")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. Vá para o final do URL no endereço e introduza `Here is my c.v. for the programmer job`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver as expressões `ApplyForJob`.

```JSON
{
  "query": "Here is my c.v. for the programmer job",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.9826467
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.9826467
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0218927357
    },
    {
      "intent": "MoveEmployee",
      "score": 0.007849265
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00349470088
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00348804821
    },
    {
      "intent": "None",
      "score": 0.00319909188
    },
    {
      "intent": "FindForm",
      "score": 0.00222647213
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00211193133
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00172086991
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00138010911
    }
  ],
  "entities": [
    {
      "entity": "programmer",
      "type": "Job",
      "startIndex": 24,
      "endIndex": 33,
      "score": 0.5230502
    }
  ]
}
```

## <a name="names-are-tricky"></a>Os nomes são complicados
A aplicação LUIS encontrou a intenção correta com um índice elevado de confiança e extraiu o nome do trabalho, mas os nomes são complicados. Experimente a expressão `This is the lead welder paperwork`.  

No seguinte JSON, o LUIS responde com a intenção correta, `ApplyForJob`, mas não extraiu o nome do trabalho `lead welder`. 

```JSON
{
  "query": "This is the lead welder paperwork.",
  "topScoringIntent": {
    "intent": "ApplyForJob",
    "score": 0.468558252
  },
  "intents": [
    {
      "intent": "ApplyForJob",
      "score": 0.468558252
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0102701457
    },
    {
      "intent": "MoveEmployee",
      "score": 0.009442534
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00639619166
    },
    {
      "intent": "None",
      "score": 0.005859333
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.005087704
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00315379258
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00259344373
    },
    {
      "intent": "FindForm",
      "score": 0.00193389168
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.000420796918
    }
  ],
  "entities": []
}
```

Uma vez que um nome pode ser qualquer coisa, o LUIS prevê as entidades com maior exatidão se tiver uma lista de expressões e palavras para melhorar o sinal.

## <a name="to-boost-signal-add-jobs-phrase-list"></a>Para melhorar o sinal, adicione uma lista de expressões de trabalho
Abra o ficheiro [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) no repositório do Github LUIS-Samples. A lista contém mais de mil expressões e palavras relacionadas com trabalho. Dê uma vista de olhos à lista para encontrar palavras relacionadas com trabalho que são relevantes para si. Se as suas palavras ou expressões não estiverem na lista, adicione-as.

1. Na secção **Build** (Criar) da aplicação LUIS, selecione **Phrase lists** (Listas de expressões), que se encontra no menu **Improve app performance** (Melhorar o desempenho da aplicação).

    [![](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png "Captura de ecrã do botão de navegação à esquerda Listas de expressões realçado")](media/luis-quickstart-primary-and-secondary-data/hr-select-phrase-list-left-nav.png#lightbox)

2. Selecione **Create new phrase list** (Criar nova lista de expressões). 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png "Captura de ecrã do botão Criar nova lista de expressões realçado")](media/luis-quickstart-primary-and-secondary-data/hr-create-new-phrase-list.png#lightbox)

3. Atribua à nova lista de expressões o nome `Jobs` e copie a lista de jobs-phrase-list.csv para a caixa de texto **Values** (Valores). Selecione Enter. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Captura de ecrã do pop-up da caixa de diálogo Criar nova lista de expressões")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Se pretender adicionar mais palavras à lista de expressões, reveja os **Valores Relacionados** e adicione os que são relevantes. 

4. Selecione **Save** (Guardar) para ativar a lista de expressões.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Captura de ecrã do pop-up da caixa de diálogo Criar nova lista de expressões com palavras na caixa de valores da lista de expressões")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. [Prepare](#train-the-luis-app) e [publique](#publish-the-app-to-get-the-endpoint-URL) a aplicação novamente para utilizar a lista de expressões.

6. Repita a consulta no ponto final com a mesma expressão: `This is the lead welder paperwork.`

    A resposta JSON inclui a entidade extraída:

    ```JSON
    {
        "query": "This is the lead welder paperwork.",
        "topScoringIntent": {
            "intent": "ApplyForJob",
            "score": 0.920025647
        },
        "intents": [
            {
            "intent": "ApplyForJob",
            "score": 0.920025647
            },
            {
            "intent": "GetJobInformation",
            "score": 0.003800706
            },
            {
            "intent": "Utilities.StartOver",
            "score": 0.00299335527
            },
            {
            "intent": "MoveEmployee",
            "score": 0.0027167045
            },
            {
            "intent": "None",
            "score": 0.00259556063
            },
            {
            "intent": "FindForm",
            "score": 0.00224019377
            },
            {
            "intent": "Utilities.Stop",
            "score": 0.00200693542
            },
            {
            "intent": "Utilities.Cancel",
            "score": 0.00195913855
            },
            {
            "intent": "Utilities.Help",
            "score": 0.00162656687
            },
            {
            "intent": "Utilities.Confirm",
            "score": 0.0002851904
            }
        ],
        "entities": [
            {
            "entity": "lead welder",
            "type": "Job",
            "startIndex": 12,
            "endIndex": 22,
            "score": 0.8295959
            }
        ]
    }
    ```

## <a name="phrase-lists"></a>Listas de expressões
A adição da lista de expressões melhorou o sinal das palavras na lista, mas **não** é utilizada como uma correspondência exata. A lista de expressões tem vários trabalhos com a primeira palavra `lead` e também tem o trabalho `welder`, mas não tem o trabalho `lead welder`. Esta lista de expressões para trabalhos pode não estar completa. À medida que [revê as expressões de ponto final](label-suggested-utterances.md) com regularidade e encontra outras palavras relacionadas com trabalho, adicione-as à sua lista de expressões. Em seguida, volte a preparar e a publicar a aplicação.

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com uma entidade simples e uma lista de expressões e palavras, identificou uma intenção de consulta de linguagem natural e devolveu os dados do trabalho. 

Agora, o seu chatbot tem informação suficiente para determinar a ação principal de candidatar-se a um trabalho e um parâmetro dessa ação, cujo trabalho é referenciado. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para utilizar uma API de terceiros para enviar as informações do trabalho para um representante dos Recursos Humanos. Se existirem outras opções programáticas para o bot ou a aplicação de chamada, o LUIS não executa essas opções. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **As minhas aplicações** no menu do canto superior esquerdo. Selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar uma entidade keyphrase criada previamente](luis-quickstart-intent-and-key-phrase.md)