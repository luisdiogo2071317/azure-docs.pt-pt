---
title: Como utilizar entradas alternativas com conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar entradas alternativas com Learner conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2214436b193932e5b3b80c190f7754a0436b7ed8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354121"
---
# <a name="how-to-use-alternative-inputs"></a>Como utilizar entradas alternativas

Este tutorial mostra como utilizar o campo "entradas alternativo" para a entrada de utilizador na interface de ensino.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
"Entradas alternativas" são utterances de utilizador alternativas que o utilizador pode ter consiga aceder tal num determinado ponto numa caixa de diálogo formação. Entradas alternativas permitem-lhe especificar mais compactly variações do que um utilizador pode dizer, sem ter de lista cada variação numa caixa de diálogo formação separado.

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza AlternativeInputs. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza cidade.
3. Clique em Criar.

### <a name="create-three-actions"></a>Criar três ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'que cidade pretende?'.
3. Na Disqualifying entidades, introduza $city.
3. Clique em Criar

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, ação de novo.
3. Em resposta, escreva 'Meteorologia no $city é provavelmente sunny'.
4. Entidades necessárias, introduza $city.
4. Clique em Criar.

Crie a ação terceira:

1. Clique em ações, em seguida, ação de novo.
3. Em resposta, escreva 'Tente pedir a meteorologia'.
    - Isto seria em resposta à pergunta do utilizador, tais como "que pode o sistema?"
4. Na Disqualifying entidades, introduza $city.
4. Clique em Criar

Agora, tem três ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva 'o que é a meteorologia'.
3. Clique em ações de pontuação e selecione 'que cidade pretende?'
2. Introduza 'denver'.
3. Faça duplo clique no 'denver' e selecione cidade.
    - Isto marca-a como uma entidade de cidade.
5. Clique em ações de pontuação
    - Tenha em atenção que denver agora está presente na entidade cidade. 
6. Selecione 'Meteorologia no $city é provavelmente sunny'.
7. Clique em concluído de ensino.

Outro exemplo caixa de diálogo adicione:

1. Clique em nova ação, em seguida, caixa de diálogo de formação novo.
2. Escreva 'o que poderá fazer?'.
3. Clique em ações de pontuação e selecione 'Tente pedir a meteorologia'
2. Introduza 'Novidades Meteorologia em seattle'.
3. Faça duplo clique em "seattle" e selecione cidade.
    - Isto marca-a como uma entidade de cidade.
5. Clique em ações de pontuação
    - Tenha em atenção que seattle agora está presente na entidade cidade. 
6. Selecione 'Meteorologia no $city é provavelmente sunny'.
7. Clique em concluído de ensino.

Vamos ver o que acontece se o utilizador indica algo semanticamente semelhante para o procedimento acima:

1. Clique em nova ação, em seguida, caixa de diálogo de formação novo.
2. Tipo de 'Ajuda'.
3. Clique em ações de pontuação.
    - Tenha em atenção que as pontuações das classificações para as dois potenciais respostas são muito fechar. Isto indica-no que modelo é confundido sobre os limites entre as duas ações.
6. Clique em abandono ensino e confirme.

![](../media/tutorial8_closescores.png)

Neste caso, iria ajudar para adicionar entradas alternativas para caixas de diálogo. Pode adicioná-los tal como estão a fazer de ensino. Também pode voltar atrás e adicioná-los mais tarde.

2. Clique em "Que poderá fazer?" nas caixas de diálogo de formação.
2. Na caixa de diálogo, clique em "que poderá fazer?" para o selecionar.
    1. No painel direito, em deteção de entidade, na entrada alternativa adicionar introduza alguns alternativas:
    1. Introduza "quais são as minhas opções?"
    2. Introduza 'Informar-me minhas opções'.
    3. Introduza 'Ajuda'
    1. Clique em submeter as alterações.


![](../media/tutorial8_helpalternates.png)

2. Agora, clique em sobre o 'que é a meteorologia em seattle'.
    1. Adicionar alternativo de entrada, introduza 'previsão para seattle'.
    2. Faça duplo clique em "seattle" e selecione cidade. Tenha em atenção que as entidades para entradas alternativas devem estar presente e tem o mesmo conjunto de entidades. É adequado se o conteúdo a entites for diferente.
    3. Adicionar alternativo de entrada, introduza 'será-rain hoje em denver'.
    4. Clique em 'denver' e selecione cidade.
    5. Clique em submeter as alterações e efetuadas.


Vamos adicionar entradas alternativas para a caixa de diálogo primeiro:

1. Clique em formação caixas de diálogo.
2. Clique na caixa de diálogo começados por 'que é a meteorologia'.
2. Clique para selecionar a opção 'o que é a meteorologia' no painel esquerdo:
    1. Adicionar alternativo de entrada, introduza 'Meteorologia previsão'.
    2. Introduza "será-rain?"
    3. Clique em submeter alterações.
4. Clique para selecionar 'denver' no painel esquerdo:
    1. Adicionar alternativo de entrada, introduza 'para denver'.
    2. Introduza 'previsão para austin'.
        - Tenha em atenção que o frase completa é realçado. Clique no frase, então o x vermelho. Em seguida, selecione austin e clique em cidade.
        - Clique em submeter alterações
    1. Clique em concluído que fará com que o modelo para a reparametrização dos.

![](../media/tutorial8_altcities.png)

Vamos tentar as variações:

1. Clique em nova caixa de diálogo de formação.
2. Tipo de 'que está capacidades'.
3. Clique em ações de pontuação.
    - Tenha em atenção que as pontuações estão agora mais decisive na próxima ação que indica o certainty do modelo.
2. Selecione 'Try solicitando Meteorologia'.
6. Clique em concluído ensino

Tem agora visualizadas entradas como alternativas, podem ser utilizadas para indicar outros aspetos, o utilizador poderá ter consiga aceder tal. Ajudam a evitar a criação de várias caixas de diálogo, que, em muitos aspetos, são os mesmos, por collapsing-los para uma única caixa de diálogo e que o utilizador pode indicar a enumerar.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Caixas de diálogo de registo](./9-log-dialogs.md)
