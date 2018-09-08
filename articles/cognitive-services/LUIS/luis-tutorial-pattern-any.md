---
title: Tutorial sobre como utilizar a entidade de pattern.any para melhorar as previsões de LUIS – Azure | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Neste tutorial, utilize a entidade de pattern.any para melhorar as previsões de intenção e a entidade de LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 43f169ae11191c2e98c4538189bce781821de980
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157859"
---
# <a name="tutorial-improve-app-with-patternany-entity"></a>Tutorial: Melhorar a aplicação com pattern.any entidade

Neste tutorial, utilize a entidade de pattern.any para aumentar a predição de intenção e entidade.  

> [!div class="checklist"]
* Saiba quando e como utilizar pattern.any
* Criar padrão que utiliza pattern.any
* Como verificar as melhorias de predição

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de recursos humanos do [padrão de funções](luis-tutorial-pattern-roles.md) tutorial, [importar](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no [LUIS](luis-reference-regions.md#luis-website) Web site. A aplicação para importar se encontra no [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-roles-HumanResources.json) repositório do GitHub.

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `patt-any`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="the-purpose-of-patternany"></a>A finalidade de pattern.any
A entidade de pattern.any permite-lhe localizar dados de forma livre onde o texto da entidade dificulta a determinar o fim da entidade do restante da expressão. 

Esta aplicação de recursos humanos ajuda os funcionários encontrar formulários de empresa. Formulários foram adicionados no [tutorial de expressão regular](luis-quickstart-intents-regex-entity.md). Os nomes de formulário desse tutorial utilizada uma expressão regular para extrair um nome de formulário que foi bem formatado como os nomes de formulário em negrito na tabela de expressão a seguir:

|Expressão|
|--|
|Onde está **HRF 123456**?|
|Quem é o autor **HRF 123234**?|
|**HRF 456098** está publicada em francês?|

No entanto, cada formulário tem um nome formatado, utilizado na tabela anterior, bem como um nome amigável, como `Request relocation from employee new to the company 2018 version 5`. 

Expressões com o nome amigável do formulário ter o seguinte aspeto:

|Expressão|
|--|
|Onde está **solicitar reposicionamento de funcionário novo para a versão de empresa 2018 5**?|
|Quem é o autor **"Pedir reposicionamento de funcionário novo para a versão de empresa 2018 5"**?|
|**Pedir o reposicionamento de funcionário novo para a versão de empresa 2018 5** está publicada em francês?|

O comprimento variado inclui expressões que podem confundir o LUIS sobre onde termina a entidade. Utilizar uma entidade de Pattern.any num padrão permite-lhe especificar o início e fim do nome do formulário, para que o LUIS corretamente extrai o nome do formulário.

**Embora os padrões permitem que forneça menos expressões de exemplo, se as entidades não são detetadas, o padrão não corresponde.**

## <a name="add-example-utterances-to-the-existing-intent-findform"></a>Adicionar expressões de exemplo para a intenção existente FindForm 
Se for difícil de criar e etiquetar a entidade de FormName, remova a entidade de keyPhrase pré-criados. 

1. Selecione **crie** no painel de navegação superior, em seguida, selecione **intenções** no painel de navegação esquerda.

2. Selecione **FindForm** na lista de objetivos.

3. Adicione algumas expressões de exemplo:

    |Expressão de exemplo|
    |--|
    |Qual é a forma **o que fazer quando um fire divide no laboratório** e quem precisa de iniciar sessão depois que ler?|
    |Onde está **solicitar reposicionamento de funcionário novo para a empresa** no servidor?|
    |Quem é o autor "**pedidos de estado de funcionamento e wellness no campus da principal**" e o que é a versão mais atual?|
    |Estou procurando o formulário com o nome "**incluindo ativos físicos de pedido de movimentação de Office**". |

    Sem uma entidade de Pattern.any, seria difícil para o LUIS entender onde o título do formulário termina devido a muitas variações de nomes do formulário.

## <a name="create-a-patternany-entity"></a>Criar uma entidade de Pattern.any
A entidade de Pattern.any extrai entidades de comprimento variável. Ela só funciona num padrão porque o padrão de marca de início e no fim da entidade. Se achar que seu padrão, quando ele inclui um Pattern.any, incorretamente, utilizam entidades extrai uma [lista explícita](luis-concept-patterns.md#explicit-lists) para corrigir este problema. 

1. Selecione **entidades** na navegação à esquerda.

2. Selecione **criar nova entidade**, introduza o nome `FormName`e selecione **Pattern.any** como o tipo. Selecione **Done** (Concluído). 

    Não é possível identificar a entidade na intenção porque um Pattern.any só é válido num padrão. 

    Se pretender que os dados extraídos para incluir outras entidades, como o número ou datetimeV2, terá de criar uma entidade composta que inclui o Pattern.any, bem como o número e o datetimeV2.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Adicionar um padrão que utiliza o Pattern.any

1. Selecione **padrões** no painel de navegação esquerda.

2. Selecione o **FindForm** intenção.

3. Introduza as seguintes expressões de modelo, o que utilizar a nova entidade:

    |Expressão de modelo|
    |--|
    |Qual é a forma ["] {FormName} ["] e quem precisa de iniciar sessão depois que ler [?]|
    |Onde é ["] {FormName} ["] no servidor do [?]|
    |Quem é o autor ["] {FormName} ["] e o que é a versão mais atual [?]|
    |Estou procurando o formulário com o nome ["] {FormName} ["] [.]|

    Se quiser conta variações do formulário, como aspas simples, em vez de aspas duplas ou de um período em vez de um ponto de interrogação, crie um novo padrão para cada variação.

4. Se removeu a entidade de keyPhrase, adicione-o novamente para a aplicação. 

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testar o novo padrão para extração de dados de forma livre
1. Selecione **testar** a partir da barra superior para abrir o painel de teste. 

2. Introduza a seguinte expressão: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Selecione **Inspect** sob o resultado para ver os resultados de teste para a entidade e propósito.

    A entidade `FormName` é encontrado em primeiro lugar, em seguida, o padrão for encontrado, determinar a intenção. Se tiver um resultado de teste em que as entidades não são detetadas e, portanto, o padrão não for encontrado, terá de adicionar mais de exemplo de expressões na intenção (não o padrão).

4. Fechar o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar funções com um padrão](luis-tutorial-pattern-roles.md)