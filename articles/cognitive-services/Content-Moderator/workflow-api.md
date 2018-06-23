---
title: Moderator conteúdo do Azure - fluxos de trabalho de moderação interrupção | Microsoft Docs
description: Utilize os fluxos de trabalho com moderação de interrupção de conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/04/2018
ms.author: sajagtap
ms.openlocfilehash: 079fcd119f1536f9e76ca57fccc76538b3c3ed78
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351865"
---
# <a name="moderation-workflows"></a>Fluxos de trabalho de moderação interrupção

Conteúdo Moderator inclui ferramentas e APIs para gerir fluxos de trabalho. Utilizar fluxos de trabalho com o [operações da API tarefa rever](review-api.md) para automatizar a criação de revisão humanos no ciclo com base nos limiares e políticas de conteúdo.

A API de revisão oferece as seguintes opções para incluir supervisão humana no seu processo de moderação de interrupção de conteúdo:

1. O **tarefa** operações para iniciar a máquina assistida moderação de interrupção e humanos rever criação como um passo.
1. O **rever** operações para humanos rever criação, fora do passo de moderação interrupção.
1. O **fluxo de trabalho** operações para a gestão de fluxos de trabalho que automatizam a análise com limiares para rever a criação.

Este artigo abrange o **fluxo de trabalho** operações. Leia o [tarefas e revê](review-api.md) descrição geral para saber mais sobre a moderação de interrupção conteúda tarefas e revê.

A verificar o de **predefinido** fluxo de trabalho é a melhor forma de começar a utilizar em fluxos de trabalho de compreender na Moderator conteúdo.

## <a name="your-first-workflow"></a>O fluxo de trabalho primeiro

O fluxo de trabalho primeiro vem agrupado com o seu [rever a equipa de ferramenta](https://contentmoderator.cognitive.microsoft.com/). Inscrever-se se não o fez, já.

Navegue para o [rever fluxos de trabalho da ferramenta](Review-Tool-User-Guide/Workflows.md) ecrã no separador Definições. Verá um **predefinido** fluxo de trabalho conforme mostrado na imagem seguinte:

![Conteúdo Moderator os fluxos de trabalho](Review-Tool-User-Guide/images/2-workflows-1.png)

### <a name="open-the-default-workflow"></a>Abrir o fluxo de trabalho predefinido

Utilize o **editar** opção para abrir a página de edição, conforme mostrado na imagem seguinte fluxo de trabalho: ![o fluxo de trabalho do conteúdo Moderator predefinido](images/default-workflow-listed.PNG)

### <a name="the-designer-view"></a>A vista designer

Pode ver o **Designer** separador do fluxo de trabalho. A vista de designer mostra os seguintes passos:

1. O **condição** do fluxo de trabalho a ser avaliada. Neste caso, as fluxo de trabalho chamadas Moderator o conteúdo imagem de API e verifica se o `isAdult` saída for igual a `true`.
1. O **ação** para ser executada se a condição for satisfeita. Neste caso, o fluxo de trabalho cria uma revisão na ferramenta de rever o `isAdult` o resultado é `true`.

![Conteúdo Moderator predefinido fluxo de trabalho - designer](images/default-workflow-designer.png)

### <a name="the-json-view"></a>A vista JSON

Selecione o **JSON** separador para ver a definição de JSON do fluxo de trabalho.

    {
        "Type": "Logic",
        If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="key-learning"></a>Learning chave

Os fluxos de trabalho no conteúdo Moderator são fáceis de configurar e flexível. Se o estruturador incorporado não cumpre os requisitos, escrever a definição de fluxo de trabalho no **JSON** formato. Em seguida, utilize a definição de JSON com o [API de fluxo de trabalho](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) para criar e gerir o fluxo de trabalho a partir da sua aplicação.

## <a name="define-a-custom-workflow"></a>Definir um fluxo de trabalho personalizado

Capacidades de fluxo de trabalho de conteúdo Moderator permitem definir e utilizar fluxos de trabalho personalizados. Utilize o [Rever ferramenta os fluxos de trabalho procedimentos](Review-Tool-User-Guide/Workflows.md) artigo para definir um fluxo de trabalho personalizado. Este fluxo de trabalho utiliza a capacidade de OCR conteúdo Moderator para extraia o texto a partir de uma imagem de exemplo. Em seguida, cria uma revisão na ferramenta de revisão.

### <a name="the-sample-image"></a>A imagem de exemplo

Guardar o [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) para disco local. Esta imagem é necessário para o exercício.

### <a name="the-designer-view"></a>A vista designer

Selecione o **Designer** separador e [tutorial de criação de fluxo de trabalho](Review-Tool-User-Guide/Workflows.md) para definir um fluxo de trabalho personalizado. A imagem seguinte mostra o estruturador **condição** vista. Consulte o tutorial para ver o resto dos passos.

![Conteúdo Moderator - condição de fluxo de trabalho](Review-Tool-User-Guide/images/ocr-workflow-step-2-condition.PNG)

### <a name="the-json-view"></a>A vista JSON

Selecione o **JSON** separador para ver a seguinte definição JSON do fluxo de trabalho personalizado. Repare como o **depois de se** declarações na definição do JSON correspondem aos passos que definiu a utilizar a vista de estruturador.

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

### <a name="workflow-result"></a>Resultado do fluxo de trabalho

Depois de testar o fluxo de trabalho a partir do ecrã de fluxos de trabalho, consulte o seguinte é criado. Navegue para o **imagem** separador em **rever** para ver a revisão.
O fluxo de trabalho criado a revisão porque a condição primária testado positivo quanto à presença de texto. Reveja também realçada o **`a`** tag na revisão de imagem.

![Conteúdo Moderator - saída de fluxo de trabalho simples](images/ocr-sample-image-workflow1.PNG)


## <a name="advanced-workflow-with-combination"></a>Fluxo de trabalho avançado com combinação

### <a name="the-sample-image"></a>A imagem de exemplo

Utilizar o mesmo [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) que foi utilizada na secção anterior.

No entanto, neste vez aproximadamente, altere a condição primária para uma combinação de verificações de dois. Para além de verificar para texto, verifique se o texto tem quaisquer profanity. O fluxo de trabalho cria uma revisão de se encontrar texto **e** Deteta profanity no mesmo.

### <a name="the-designer-view"></a>A vista designer

Para alterar o **condição** para um **combinação**, modifique o fluxo de trabalho. A imagem seguinte mostra a nova vista que vê no designer.

![Conteúdo Moderator - condição de modificação do fluxo de trabalho](images/ocr-workflow-2-designer.PNG)

### <a name="the-json-view"></a>A vista JSON

Selecione o **JSON** separador para ver a seguinte definição JSON do fluxo de trabalho personalizado modificado. Repare como o **depois de se** declarações na definição do JSON correspondem aos novos passos adicionados ao fluxo de trabalho.

    {
        "Type": "Logic",
        "If": {
        "Left": {
            "ConnectorName": "moderator",
            "OutputName": "hasText",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Right": {
            "ConnectorName": "moderator",
            "OutputName": "text.HasProfanity",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition",
            "AlternateInput": "moderator.ocrText"
            },
        "Combine": "AND",
        "Type": "Combine"
        },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": [
            {
                "Tag": "a",
                "IfCondition": {
                    "ConnectorName": "moderator",
                    "OutputName": "hasText",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                }
            }
            ]
        }
        ],
        "Type": "Actions"
        }
    }

    
### <a name="workflow-result"></a>Resultado do fluxo de trabalho

Depois de testar o fluxo de trabalho novamente, encontrará que não é criado nenhum revisão. Para confirmar a ausência de qualquer revisão, navegue para o **imagem** separador em **rever**.
O fluxo de trabalho não conseguiu criar a revisão porque não conseguiu detetar profanity no texto extraído.

![Conteúdo Moderator - saída de fluxo de trabalho modificado](images/ocr-workflow-2-result.PNG)


## <a name="the-workflow-api"></a>O fluxo de trabalho API

O [operações de fluxo de trabalho](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) fornecer a interface de programação às funcionalidades de fluxo de trabalho. Criar fluxos de trabalho, obter detalhes de fluxo de trabalho e atualizar as definições de fluxo de trabalho utilizando a API de fluxo de trabalho.

### <a name="get-all-workflow-details"></a>Detalhes de fluxo de trabalho GET [todas]

O **Get de fluxo de trabalho** operação aceita as seguintes entradas:

- **equipa**: O ID de equipa que criou quando configurou o [rever conta ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do fluxo de trabalho. Utilize `default` lugar.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

Se a operação for bem sucedida, o **estado de resposta** é `200 OK` e **conteúdo da resposta** caixa apresenta a definição de fluxo de trabalho no formato JSON.
Para obter mais informações, leia o [guia de introdução do fluxo de trabalho API consola](try-review-api-job.md).

### <a name="create-or-update-workflow"></a>Criar ou atualizar o fluxo de trabalho

A operação de criação e atualização permite a criação de fluxo de trabalho a partir da API.

O **criar o fluxo de trabalho ou atualização** operação aceita as seguintes entradas:

- **equipa**: O ID de equipa que criou quando configurou o [rever conta ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do fluxo de trabalho. Utilize `default` lugar.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

Se a operação for bem sucedida, o **estado de resposta** é `200 OK` e **conteúdo da resposta** caixa apresenta `true`. Para obter mais informações, [testar o `Create` operação](try-review-api-job.md).

## <a name="next-steps"></a>Passos Seguintes

Para saber como criar fluxos de trabalho personalizados, consulte o [rever tutorial de fluxo de trabalho da ferramenta](Review-Tool-User-Guide/Workflows.md). 

Testar o [consola de API de fluxo de trabalho](try-review-api-job.md) e utilizar os exemplos de código da REST API. 

Por último, utilize os fluxos de trabalho personalizados com o **tarefa** operações como shon no [consola de API de tarefa](try-review-api-job.md) e [início rápido de .NET de tarefas](moderation-jobs-quickstart-dotnet.md).
