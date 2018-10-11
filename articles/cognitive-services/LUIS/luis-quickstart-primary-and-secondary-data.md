---
title: 'Tutorial 7: Entidade simples com lista de expressões no LUIS'
titleSuffix: Azure Cognitive Services
description: Extrair dados de aprendizagem automática de uma expressão
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: e0cdda629ddded121a424af61377c04ee8d958d3
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867549"
---
# <a name="tutorial-7-extract-names-with-simple-entity-and-phrase-list"></a>Tutorial 7: Extrair nomes com entidade simples e lista de expressões

Neste tutorial, irá extrair dados de aprendizagem automática do nome do cargo a partir de uma expressão com a entidade **Simple** (Simples). Para aumentar a precisão de extração, adicione uma lista de expressões de termos específicos da entidade simples.

Este tutorial adiciona uma nova entidade simples para extrair o nome do cargo. O objetivo da entidade simples nesta aplicação LUIS é ensinar ao LUIS o que é um nome de trabalho e onde pode ser encontrado numa expressão. A parte da expressão que corresponde ao nome do cargo pode variar de expressão para expressão com base na escolha de palavras e no comprimento da expressão. O LUIS precisa de exemplos de nomes de cargos em todas as intenções que utilizam nomes de cargos.  

A entidade simples é uma boa opção para este tipo de dados quando:

* Os dados são um único conceito.
* Os dados não são bem formatados, como uma expressão regular.
* Os dados não são comuns, como uma entidade pré-concebida do número de telefone ou de dados.
* Os dados não correspondem exatamente a uma lista de palavras conhecidas, como uma entidade de lista.
* Os dados não contêm outros itens de dados, como uma entidade composta ou hierárquica.

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar uma entidade simples para extrair tarefas da aplicação
> * Adicionar a lista de expressões para melhorar o sinal das palavras da tarefa
> * Preparar 
> * Publicar 
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente

Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-composite-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `simple`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL.

## <a name="simple-entity"></a>Entidade simples
A entidade simples deteta um único conceito de dados contido em palavras ou expressões.

Considere as seguintes expressões de um chatbot:

|Expressão|Nome da tarefa passível de extração|
|:--|:--|
|Quero candidatar-me ao novo trabalho de contabilidade.|contabilidade|
|Submeter o meu currículo para o novo cargo de engenharia.|engenharia|
|Preencher a candidatura para o trabalho 123456|123456|

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

Esta aplicação LUIS tem nomes de trabalhos em várias intenções. Ao identificar estas palavras nas expressões de todas as intenções, o LUIS aprende mais sobre o nome de um cargo e onde pode ser encontrado nas expressões.

Depois de as entidades serem marcadas nas expressões de exemplo, é importante adicionar uma lista de expressões para aumentar o sinal da entidade simples. Uma lista de expressões **não** é utilizada como uma correspondência exata e não precisa de ser todos os valores possíveis esperados. 

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Na página **Intents** (Intenções), selecione a intenção **ApplyForJob**. 

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

## <a name="label-entity-in-example-utterances"></a>Etiquetar a entidade nas expressões de exemplo

Etiquetar ou _marcar_ a entidade mostra ao LUIS onde está a entidade nas expressões de exemplo.

1. Selecione **Intents** (Intenções) no menu esquerdo.

2. Selecione **GetJobInformation** na lista de intenções. 

3. Identifique os trabalhos nas expressões de exemplo:

    |Expressão|Entidade de trabalho|
    |:--|:--|
    |Existe algum trabalho em bases de dados?|bases de dados|
    |Procuro um novo emprego com responsabilidades em contabilidade|contabilidade|
    |Que vagas estão disponíveis para engenheiros sénior?|engenheiros sénior|

    Existem outras expressões de exemplo, mas não contêm palavras relacionadas com trabalhos.

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter as intenções e as entidades do ponto final 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `Here is my c.v. for the programmer job`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver as expressões `ApplyForJob`.

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
    
    O LUIS encontrou a intenção correta, **ApplyForJob**, e extraiu a entidade correta, **Job**, com um valor de `programmer`.


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

## <a name="to-boost-signal-add-phrase-list"></a>Para melhorar o sinal, adicione uma lista de expressões

Abra o ficheiro [jobs-phrase-list.csv](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/job-phrase-list.csv) no repositório do Github LUIS-Samples. A lista contém mais de mil expressões e palavras relacionadas com trabalho. Dê uma vista de olhos à lista para encontrar palavras relacionadas com trabalho que são relevantes para si. Se as suas palavras ou expressões não estiverem na lista, adicione-as.

1. Na secção **Build** (Criar) da aplicação LUIS, selecione **Phrase lists** (Listas de expressões), que se encontra no menu **Improve app performance** (Melhorar o desempenho da aplicação).

2. Selecione **Create new phrase list** (Criar nova lista de expressões). 

3. Atribua à nova lista de expressões o nome `Job` e copie a lista de jobs-phrase-list.csv para a caixa de texto **Values** (Valores). Selecione Enter. 

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png "Captura de ecrã do pop-up da caixa de diálogo Criar nova lista de expressões")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-1.png#lightbox)

    Se pretender adicionar mais palavras à lista de expressões, reveja os **Valores Relacionados** e adicione os que são relevantes. 

4. Selecione **Save** (Guardar) para ativar a lista de expressões.

    [![](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png "Captura de ecrã do pop-up da caixa de diálogo Criar nova lista de expressões com palavras na caixa de valores da lista de expressões")](media/luis-quickstart-primary-and-secondary-data/hr-create-phrase-list-2.png#lightbox)

5. [Prepare](#train) e [publique](#publish) a aplicação novamente para utilizar a lista de expressões.

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

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, a aplicação Recursos Humanos utiliza uma entidade de aprendizagem automática simples para localizar nomes de cargos nas expressões. Como os nomes de cargos podem ter uma grande variedade de palavras ou expressões, a aplicação precisou de uma lista de expressões para melhorar as palavras dos nomes de cargos. 

> [!div class="nextstepaction"]
> [Adicionar uma entidade keyphrase criada previamente](luis-quickstart-intent-and-key-phrase.md)