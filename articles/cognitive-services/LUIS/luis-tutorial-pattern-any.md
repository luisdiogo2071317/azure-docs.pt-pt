---
title: 'Tutorial 5: Entidade de Pattern.any para texto de forma livre'
titleSuffix: Azure Cognitive Services
description: Utilize a entidade de pattern.any para extrair dados de expressões em que as expressões é bem formatada e em que o fim dos dados pode ser facilmente confundido com as palavras da expressão.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: dce75710137f4d4160cb2f55f856066c7c93ac78
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45628988"
---
# <a name="tutorial-5-extract-free-form-data"></a>Tutorial: 5. Extrair dados de forma livre

Neste tutorial, utilize a entidade de pattern.any para extrair dados de expressões em que as expressões são bem formatadas e em que o fim dos dados pode ser facilmente confundido com as palavras da expressão. 

A entidade de pattern.any permite-lhe localizar dados de forma livre onde o texto da entidade dificulta a determinar o fim da entidade do restante da expressão. 

Esta aplicação de recursos humanos ajuda os funcionários encontrar formulários de empresa. 

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

O comprimento variado inclui palavras que podem confundir o LUIS sobre onde termina a entidade. Utilizar uma entidade de Pattern.any num padrão permite-lhe especificar o início e fim do nome do formulário, para que o LUIS corretamente extrai o nome do formulário.

|Exemplo de expressão de modelo|
|--|
|Onde é {FormName} [?]|
|Quem criados {FormName} [?]|
|{FormName} está publicada em francês [?]|

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Utilizar o tutorial de aplicação existente
> * Adicionar expressões de exemplo para uma entidade existente
> * Criar entidade Pattern.any
> * Criar padrão
> * Preparar
> * Novo padrão de teste

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continuar com a aplicação criada no tutorial última, com o nome **RecursosHumanos**. 

Se não tiver a aplicação de RecursosHumanos no tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-roles-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Do **Manage** na secção a **versões** separador, clonar a versão e nomeie- `patt-any`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter quaisquer carateres que não são válidos num URL.

## <a name="add-example-utterances"></a>Adicionar expressões de exemplo 
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

Este tutorial adicionado expressões de exemplo para um objetivo existente, em seguida, criou um novo Pattern.any para o nome do formulário. Em seguida, o tutorial criou um padrão para a intenção existente com as novas expressões de exemplo e uma entidade. O teste interativo mostrou que o padrão e qual a sua intenção foram previstos porque a entidade foi encontrada. 

> [!div class="nextstepaction"]
> [Saiba como utilizar funções com um padrão](luis-tutorial-pattern-roles.md)