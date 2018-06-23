---
title: Como ensinar com conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como ensinar com Learner conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 639fea64fc8eeb2c1f6e6240c4eb26efc68febbd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353971"
---
# <a name="how-to-teach-with-conversation-learner"></a>Como ensinar com conversação Learner 

Este documento explica o que indica que está ciente de conversação Learner e descreve como aprende.  

Pode ser decomposed ensino em dois passos distintos: a extração de entidade e seleção de ação.

## <a name="entity-extraction"></a>Extração de entidade

Nos bastidores, utiliza a conversação Learner [LUIS](https://www.luis.ai) para extração de entidade.  Se estiver familiarizado com LUIS, o que ocorrer se aplica a extração de entidade na conversação Learner.

Os modelos de extração de entidade sabem o *conteúdo* e *contexto* dentro de um utterance de utilizador.  Por exemplo, se a palavra "Seattle" tem sido identificada como uma cidade no um utterance como "Qual é a meteorologia em Seattle", extração de entidade é capaz de reconhecer os mesmos conteúdos ("Seattle") como uma cidade no outro utterance, tais como "População de Coimbra", mesmo se a utterances são muito diferentes.  Por outro lado, se tiver de "Francis" foi reconhecida como um nome de "Agenda uma reunião com Francis", em seguida, um novo nome unseen anteriormente pode ser reconhecido num contexto semelhante, como "Configurar uma reunião com Round Robin".  Aprendizagem infere quando aguardem conteúdo, contexto ou ambos, com base nos exemplos de formação.

Atualmente, extração de entidade só está ciente do conteúdo de utterance atual.  Ao contrário de seleção de ação (abaixo), não está ciente do histórico de caixa de diálogo como activa anterior do sistema, anterior activa de utilizador ou entidades anteriormente reconhecidas.  Como resultado, o comportamento de extração de entidade é "partilhado" em todos os utterances.  Por exemplo, se o utterance de utilizador "Quero Apple" tiver "Apple" etiqueta como tipo de entidade "Acessíveis" um utterance de utilizador, o modelo de extração de entidade será esperar que este utterance ("quero Apple") deve ter sempre "Apple" identificada como "Acessíveis".

Se a extração de entidade não está a comportar conforme esperado, seguem-se responsabilidade possíveis:

- A primeira coisa que tentar consiste em adicionar mais exemplos de formação – particularmente exemplos revelar contexto típico de entidade (envolvente palavras) ou de exceções
- Considere adicionar uma propriedade de "Entidade esperado" a uma ação, se apropriado.  Consulte o tutorial sobre entidades esperado para obter mais detalhes.
- Embora seja possível adicionar o processamento manual `EntityExtractionCallback` para extrair as entidades com o código, esta é a abordagem recomendada pelo menos, porque não irá beneficiar de melhorias no machine learning como evoluiu do seu sistema.

## <a name="action-selection"></a>Seleção de ação

Seleção de ação utiliza uma rede neuronal periódica, que demora a todas como entrada do histórico de conversações.  Assim, a seleção de ação é um processo com monitorização de estado que tem conhecimento de utterances anterior do utilizador, os valores de entidade e utterances de sistema.  

Alguns sinais naturalmente são preferencial pelo processo de aprendizagem.  Por outras palavras, se conversação Learner podem explicar uma decisão de seleção de ação com mais de "preferenciais" sinais, este irá; Se não é possível, em seguida, irá utilizar menos sinais "preferenciais".

Eis uma tabela que mostra todas as sinais na conversação Learner e aqueles que são utilizados por seleção de ação.  Tenha em atenção que a ordem do word na utterances de utilizador é ignorada.

Sinal | Preferência (1 = topo das preferências) | Notas
--- | --- | --- 
Ação de sistema, por sua vez anterior | 1 | 
Apresentam as entidades por sua vez atual | 1 | 
Se esta é a primeira vez | 1 |
Correspondência exata de palavras no utterance do utilizador atual | 2 | 
É semelhante significado palavras no utterance do utilizador atual | 3 | 
Ações de sistema antes de ativado anterior | 4 |
Entidades está presente na activa antes de ativar atual | 4 | 
Utterances do utilizador antes de ativar atual | 5 | 

Tenha em atenção que a seleção de ação demorar o conteúdo das ações de sistema - o texto, conteúdo do cartão, ou nome da API ou comportamento – apenas a identidade da ação de sistema.  Como resultado, a alterar o conteúdo de uma ação não irá alterar o comportamento do modelo de seleção de ação.

Além disso, tenha em atenção que os conteúdos valores de entidades não são utilizados – apenas sua presença/ausência.

Se a seleção de ação não está a comportar conforme esperado, seguem-se responsabilidade possíveis:

- Adicione mais formação caixas de diálogo, particularmente as caixas de diálogo que ilustram que sinais de seleção de ação deve ser prestando especial atenção aos.  Por exemplo, se a seleção de ação deve preferir um sinal em vez de outro, conceda exemplos que apresentam o sinal preferencial está a ser o mesmo Estado e outros sinais vários.  Alguns sequências podem demorar alguns a formação caixas de diálogo para saber mais.
- Adicione "Necessária" e "Disqualifying" entidades para definições de ação.  Este limites quando as ações disponíveis e podem ser úteis para as regras de negócio rápida e alguns padrões comuns sentido. 

## <a name="updates-to-models"></a>Atualizações a modelos

Sempre que adicionar ou editar uma entidade, de ação ou caixa de diálogo de formação na IU do, isto gera um pedido para voltar preparar o modelo de extração de entidade e modelo de seleção de ação.  Este pedido está colocado uma fila e voltar a formação é efetuada no modo assíncrono.  Quando estiver disponível um novo modelo, é utilizado partir desse ponto, para a seleção de extração e a ação de entidade.  Este processo novamente training frequentemente demora cerca de 5 segundos, mas pode ser superior se o modelo é complexo, ou se a carga sobre o serviço de formação é elevada.

Uma vez formação é feita de forma assíncrona, é possível que as edições efetuadas não são imediatamente refletidas.  Se a seleção de extração ou a ação de entidade não está a comportar conforme esperado com base nas alterações efetuadas nos últimos 5-10 segundos, isto poderá ser a causa.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Os valores predefinidos e limites](./cl-values-and-boundaries.md)
