---
title: Melhorar o seu classificador com o serviço de visão personalizada - serviços cognitivos do Azure | Documentos da Microsoft
description: Saiba como melhorar a qualidade do seu classificador de serviço de visão personalizada.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 07/05/2018
ms.author: nolachar
ms.openlocfilehash: 7c6cbd996d0c35b96fde78daf391bebb36feddce
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888185"
---
# <a name="how-to-improve-your-classifier"></a>Como melhorar o seu classificador

Saiba como melhorar a qualidade do seu classificador de serviço de visão personalizada. A qualidade do seu classificador é dependente da quantidade, a qualidade e a variedade dos dados etiquetados que indicar para ele e o conjunto de dados como equilibrada é. Um bom classificador normalmente tem um conjunto de dados de treinamento equilibrada que seja representativo da qual serão submetidos para o classificador. O processo de criação de tal um classificador é muitas vezes iterativo. É comum para efetuar algumas rodadas de treinamento para alcançar os resultados esperados.

Eis os passos comuns para melhorar um classificador. Estes passos não são regras definitiva e cabal, mas heurística que irá ajudá-lo a criar um classificador melhor.

1. Primeira rodada treinamento
1. Adicionar mais imagens e o balanceamento de dados
1. Preparar novamente
1. Adicionar imagens com variadas em segundo plano, o iluminação, o tamanho do objeto, o ângulo de câmera e o estilo
1. Voltar a preparar & feed na imagem para predição
1. Examine os resultados de predição
1. Modificar os dados de treinamento existentes

## <a name="data-quantity-and-data-balance"></a>Saldo de dados e a quantidade de dados

A coisa mais importante é carregar suficiente imagens para treinar o classificador. Pelo menos 50 imagens por etiqueta para o conjunto de treinamento são recomendadas como um ponto de partida. Com menos imagens, há um risco forte que está a ser overfitting. Enquanto os seus números de desempenho podem sugerir boa qualidade, podem apresentar dificuldades em relação a dados do mundo real. O classificador com mais imagens de formação em geral, irá aumentar a precisão dos resultados de predição.

Outra consideração é que deve certificar-se de que os seus dados são balanceados. Por exemplo, ter 500 imagens para uma etiqueta e 50 imagens para outra etiqueta produzirá um conjunto de dados de treinamento desequilibrado, fazendo com que o modelo ser mais precisa ao prever uma etiqueta que o outro. Provavelmente verá melhores resultados, se mantiver, pelo menos, um rácio de 1:2 entre a etiqueta com o menor número de imagens e a etiqueta com a maioria das imagens. Por exemplo, se a etiqueta com o maior número de imagens tem 500 imagens, a etiqueta com as imagens, pelo menos, tem de ter, pelo menos, 250 imagens para treinamento.

## <a name="train-more-diverse-images"></a>Preparar as imagens mais diversificadas

Fornece imagens que representem o que irá ser submetido para o classificador durante o uso normal. Por exemplo, se estiver a treinar um classificador de "apple", seu classificador pode não ser tão preciso, se treinar fotografias de maçãs no pratos mas previsões de marca de fotografias de maçãs em árvores apenas. Incluindo diversas imagens serão Certifique-se de que o seu classificador não é medida e pode generalizar bem. Veja a seguir estão algumas formas a que se pode tornar seu treinamento definidas mais diversificadas:

__Em segundo plano:__ fornecer imagens de seu objeto à frente de diferentes planos de fundo (ou seja, frutos no prato versus frutos na matriz de acessar). Fotografias no contexto são melhores do que fotografias na frente de planos de fundo neutras que fornecem mais informações para o classificador.

![Imagem dos exemplos em segundo plano](./media/getting-started-improving-your-classifier/background.png)

__Iluminação:__ fornecer imagens com iluminação variada (ou seja, feita com exposição de flash, alta, etc.), especialmente se as imagens usadas para predição tem iluminação diferente. Também é útil incluir imagens com saturação variada, hue e brilho.

![Imagem de amostras de iluminação](./media/getting-started-improving-your-classifier/lighting.png)

__Tamanho do objeto:__ imagens forneça em que os objetos são de diversificados Capturar partes diferentes do objeto de dimensionamento. Por exemplo, uma fotografia de muitas bananas e um grande plano de um banana único. Dimensionamento diferente ajuda o classificador de generalizar melhor.

![Imagem de amostras de tamanho](./media/getting-started-improving-your-classifier/size.png)

__Ângulo da câmara:__ fornecer imagens feitas com ângulos de câmera diferentes. Se todas as suas fotografias são executadas com um conjunto de câmeras fixos (por exemplo, câmeras de vigilância), certifique-se de que atribuir uma etiqueta diferente para cada câmara, mesmo que estes capturarem os mesmos objetos para evitar overfitting - modelar relacionada com os objetos (como lampposts) como o principal recurso.

![Imagem de amostras de ângulo](./media/getting-started-improving-your-classifier/angle.png)

__Estilo:__ fornecer imagens de diferentes estilos da mesma classe (ou seja, de diferentes tipos de citrus). No entanto, se tiver imagens de objetos de estilos drasticamente diferentes (ou seja, Mickey Mouse versus uma classificação da vida real), é recomendável que identifique-los como classes separadas para melhor representam as respetivas funcionalidades distintas.

![Imagem de amostras de estilo](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Utilizar imagens submetidas para a predição

O serviço de visão personalizada armazena imagens submetidas para o ponto final de predição. Para utilizar estas imagens para melhorar o classificador, utilize os seguintes passos:

1. Para exibir imagens submetidas para o classificador, abra a [página da web de visão personalizada](https://customvision.ai), aceda ao seu projeto e selecione o __previsões__ separador. A vista predefinida mostra imagens a partir de iteração atual. Pode utilizar o __iteração__ pendente do campo para exibir imagens submetidas durante iterações anteriores.

    ![Imagem do separador de previsões](./media/getting-started-improving-your-classifier/predictions.png)

2. Coloque o cursor sobre uma imagem para ver as etiquetas que foram previstas pelo classificador. Imagens estão ordenadas para que as imagens que podem levar os maioria dos ganhos para o classificador estejam na parte superior. Para selecionar uma classificação diferente, utilize o __ordenação__ secção. Para adicionar uma imagem aos seus dados de treinamento existentes, selecione a imagem, selecione a etiqueta correta e clique em __guarde e feche__. A imagem será removida do __previsões__ e adicionados às imagens de formação. Pode vê-la ao selecionar o __imagens de formação__ separador.

    ![Imagem da página etiquetagem](./media/getting-started-improving-your-classifier/tag.png)

3. Utilize o __Train__ botão para voltar a preparar o classificador.

## <a name="visually-inspect-predictions"></a>Inspecionar visualmente previsões

Para inspecionar as previsões de imagem, selecione o __imagens de formação__ separador e, em seguida, selecione __histórico de iteração__. Imagens que são descritas com uma caixa vermelha foram previstas incorretamente.

![Imagem de histórico de iteração](./media/getting-started-improving-your-classifier/iteration.png)

Inspeção visual, às vezes, pode identificar padrões que, em seguida, pode corrigir adicionando dados de treinamento adicionais ou modificar os dados de treinamento existente. Por exemplo, um classificador para apple versus verde-limão incorretamente pode etiquetar todas as maçãs verde como limes. Poderá corrigir este problema ao adicionar e fornecer os dados de treinamento que contém imagens etiquetadas de maçãs verde.

## <a name="unexpected-classification"></a>Classificação inesperada

Por vezes, o classificador incorretamente aprende características que suas imagens têm em comum. Por exemplo, se estiver a criar um classificador de maçãs versus citrus e são fornecidas imagens de maçãs nas mãos e de citrus em brancas pratos, pode treinar o classificador para mãos versus pratos brancas em vez de maçãs versus citrus.

![Imagem da classificação inesperada](./media/getting-started-improving-your-classifier/unexpected.png)

Para corrigir este problema, utilize as orientações acima no treinamento com os mais variados imagens: fornecer imagens com diferentes ângulos, planos de fundo, tamanho do objeto, grupos e outras variantes.

## <a name="negative-image-handling"></a>Manipulação de imagem negativo

O serviço de visão personalizada oferece suporte a alguns manipulação de imagem negativo automática. No caso em que estão a criar um grape versus classificador banana e enviar uma imagem de um calçado para predição, o classificador deve classificar essa imagem como próximo de 0% para grape e banana.

Por outro lado, nos casos em que as imagens negativas são apenas uma variação das imagens utilizadas na formação, é provável que o modelo classifica as imagens negativas como uma classe etiquetada devido às semelhanças excelentes. Por exemplo, se tiver uma cor de laranja versus classificador grapefruit e alimentar numa imagem de um clementine, ele pode classificar o clementine como laranja. Isto pode ocorrer desde muitos recursos do clementine (cor, forma, textura, natural habitat, etc.) são semelhantes às de laranjas.  Se as imagens negativas dessa natureza, é recomendado para criar um ou mais separam marcas ("outros") e etiquetar negativas imagens com esta etiqueta durante o treinamento para permitir que o modelo para melhor distinção entre essas classes.

## <a name="next-steps"></a>Passos Seguintes

Saiba como pode testar imagens por meio de programação ao submetê-los para a API de predição.

> [!div class="nextstepaction"]
[Utilizar a API de predição](use-prediction-api.md)
