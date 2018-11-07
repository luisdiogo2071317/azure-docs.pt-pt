---
title: Como utilizar entradas alternativas com Aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entradas alternativas com Aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3b38cc265b4adfb301dc1165e02b17a8aa7c9589
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51252552"
---
# <a name="how-to-use-alternative-inputs"></a>Como utilizar entradas alternativas

Este tutorial mostra como utilizar o campo "entradas alternativo" para entrada do usuário na interface de ensino.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 8](https://aka.ms/cl-tutorial-08-preview)](https://aka.ms/blis-tutorial-08)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
"Entradas alternativas" são expressões de utilizador alternativas que o usuário poderia têm diz num momento específico numa caixa de diálogo de treinamento. Entradas alternativas permitem-lhe especificar mais compactly variações do que um utilizador possa dizer, sem ter de listar cada variação numa caixa de diálogo de treinamento separados.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite AlternativeInputs. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. Introduza nome da entidade, cidade.
3. Clique em Criar.

### <a name="create-three-actions"></a>Criar três ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "que cidade deseja?".
3. Nas entidades Disqualifying, introduza $city.
3. Clique em Criar

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, nova ação.
3. Em resposta, escreva "o clima em $city é provavelmente ensolarado".
4. Entidades necessárias, introduza $city.
4. Clique em Criar.

Crie a ação de terceiro:

1. Clique em ações, em seguida, nova ação.
3. Em resposta, escreva "Tentar pedindo a meteorologia".
    - Isso seria em resposta a pergunta do utilizador, tais como "o que pode o sistema?"
4. Nas entidades Disqualifying, introduza $city.
4. Clique em Criar

Agora tem três ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva 'o que é a meteorologia'.
3. Clique em ações de pontuação e selecione "que cidade deseja?"
2. Introduza 'denver'.
3. Faça duplo clique em "denver" e selecione a cidade.
    - Isto marca-o como uma entidade de cidade.
5. Clique em ações de pontuação
    - 'denver' agora está presente na entidade cidade. 
6. Selecione "o clima em $city é provavelmente ensolarado".
7. Clique em concluído ensino.

Adicione outra caixa de diálogo de exemplo:

1. Clique em nova ação, em seguida, nova caixa de diálogo de comboio.
2. Escreva "o que pode fazer?".
3. Clique em ações de pontuação e selecione "Tentar pedindo a meteorologia"
2. Introduza "O que é o clima em seattle".
3. Faça duplo clique em "seattle" e selecione a cidade.
    - Isto marca-o como uma entidade de cidade.
5. Clique em ações de pontuação
    - "seattle" agora está presente na entidade cidade. 
6. Selecione "o clima em $city é provavelmente ensolarado".
7. Clique em concluído ensino.

Vejamos o que acontece se o usuário diz algo semanticamente similar ao acima:

1. Clique em nova ação, em seguida, nova caixa de diálogo de comboio.
2. Escreva "ajuda".
3. Clique em ações de pontuação.
    - As pontuações para as duas respostas potenciais são muito parecidas. Isso nos diz que o modelo é confuso sobre os limites entre as duas ações.
6. Clique em abandono ensinando e confirme.

![](../media/tutorial8_closescores.png)

Neste caso, seria útil para adicionar entradas alternativas para caixas de diálogo. Pode adicioná-los conforme estão a fazer o ensino. Também pode voltar atrás e adicioná-los mais tarde.

2. Clique em "O que pode fazer?" nas caixas de diálogo de comboio.
2. Na caixa de diálogo, clique em "o que pode fazer?" para o selecionar.
    1. No painel direito, sob a deteção de entidade, na entrada alternativa adicionar introduza algumas alternativas:
    1. Introduza "quais são minhas opções?"
    2. Introduza "Diga-me minhas opções".
    3. Introduza "ajuda"
    1. Clique em submeter as alterações.


![](../media/tutorial8_helpalternates.png)

2. Agora, clique em "o que é o clima em seattle".
    1. Na entrada alternativa de adicionar, introduza "previsão para seattle".
    2. Faça duplo clique em "seattle" e selecione a cidade. As entidades de entradas de dados alternativas devem estar presente e tem o mesmo conjunto de entidades. Há problema se o conteúdo das entidades é diferente.
    3. Na entrada alternativa de adicionar, introduza 'será-lo de chuva hoje em denver'.
    4. Clique em "denver" e selecione a cidade.
    5. Clique em submeter as alterações e feito.


Vamos adicionar entradas alternativas para a primeira caixa de diálogo:

1. Clique em caixas de diálogo de comboio.
2. Clique na caixa de diálogo a partir do 'o que é a meteorologia'.
2. Clique para selecionar a opção 'o que é a meteorologia' no painel esquerdo:
    1. Na entrada alternativa de adicionar, introduza 'previsão meteorológica'.
    2. Introduza "será-lo de chuva?"
    3. Clique em submeter as alterações.
4. Clique para selecionar 'denver' no painel esquerdo:
    1. Na entrada alternativa de adicionar, introduza "de denver'.
    2. Introduza 'Previsão de austin'.
        - A frase completa é realçada. Clique na frase, em seguida, x vermelho. Em seguida, selecione a austin e clique em cidade.
        - Clique em submeter alterações
    1. Clique em concluído que fará com que o modelo para voltar a preparar.

![](../media/tutorial8_altcities.png)

Vamos experimentar as variações:

1. Clique em nova caixa de diálogo de comboio.
2. Escreva 'que está a capacidades'.
3. Clique em ações de pontuação.
    - As classificações estão agora mais decisivo na próxima ação que indica a certeza do modelo.
2. Selecione "Tentar solicitar a meteorologia".
6. Clique em concluído ensino

Tem agora vistas entradas como alternativas, podem ser utilizadas para indicar a outras coisas que o utilizador possa ter dito. Ajudam a evitar a criação de muitas caixas de diálogo, que, de muitas formas, são iguais, o recolhimento-los numa única caixa de diálogo e enumerar o que o utilizador pode dizer.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Caixas de diálogo de registo](./9-log-dialogs.md)
