---
title: Melhorar o classificador de utilizar o serviço de visão personalizada - serviços cognitivos do Azure | Microsoft Docs
description: Saiba como melhorar a qualidade dos seus classificador de serviço de visão personalizada.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 65b1424f259066c7d5bd6b2b508d2a4052ff0527
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352873"
---
# <a name="how-to-improve-your-classifier"></a>Como melhorar o classificador

Saiba como melhorar a qualidade dos seus classificador de serviço de visão personalizada. A qualidade dos seus classificador está dependente da qualidade dos dados identificados que fornecer-lhe. 

## <a name="train-more-varied-images"></a>Preparar imagens mais variadas

Fornecer imagens marcadas com os ângulos de diferentes, os fundos dos, tamanho do objeto, grupos de fotografias e outros variantes melhora o classificador. Fotografias no contexto são melhores do que fotografias à frente independentes dos jogadores. Inclua imagens que sejam representativas das que irá ser submetido para o classificador durante a utilização normal.

Para obter mais informações sobre como adicionar imagens, consulte o [criar um classificador](getting-started-build-a-classifier.md) documento.

> [!IMPORTANT]
> Lembre-se preparar o classificador depois de ter adicionado imagens.

## <a name="use-images-submitted-for-prediction"></a>Utilizar imagens submetidas para predição

O serviço de visão personalizada armazena imagens submetidas para o ponto final de predição. Para estas imagens são utilizadas para melhorar o classificador, utilize os seguintes passos:

1. Para ver imagens submetidas para o classificador, abra o [página web de visão personalizada](https://customvision.ai) e selecione o __Predições__ separador.

    ![Imagem de separador predições](./media/getting-started-improving-your-classifier/predictions-tab.png)

    > [!TIP]
    > A vista predefinida mostra imagens a partir da iteração atual. Pode utilizar o __iteração__ pendente de campo para ver imagens submetidas durante iterações anteriores.

2. Coloque o cursor sobre uma imagem para ver as etiquetas que foram rigor o classificador.

    > [!TIP]
    > Imagens estão ordenadas, para que as imagens que podem colocar os maioria dos ganhos o classificador estão na parte superior. Para selecionar uma ordenação diferente, utilize o __ordenação__ secção.

    Para adicionar uma imagem aos seus dados de formação, selecione a imagem, selecione a etiqueta e, em seguida, selecione __guarde e feche__. A imagem é removida do __Predições__ e adicionados às imagens de formação. Pode vê-la, selecionando o __imagens de formação__ separador.

    ![Imagem da página etiquetagem](./media/getting-started-improving-your-classifier/tag-image.png)

3. Utilize o __formação__ botão para a reparametrização do classificador.

## <a name="visually-inspect-predictions"></a>Inspecione visualmente predições

Para inspecionar os predições de imagem, selecione o __imagens de formação__ separador e, em seguida, selecione __iteração histórico__. As imagens que são descritas com uma caixa vermelha foram prever incorretamente.

![Imagem do histórico iteração](./media/getting-started-improving-your-classifier/iteration-history.png)

Inspeção visual, por vezes, pode identificar padrões que, em seguida, pode corrigir adicionando dados de preparação adicional. Por exemplo, um classificador para roses vs daises incorretamente pode Etiquetar roses brancos todos os como daises. Poderá corrigir este problema adicionando e fornecer os dados de formação que contém imagens marcadas de roses brancos.

## <a name="unexpected-classification"></a>Classificação inesperada

Por vezes, o classificador aprende características com as imagens em comum. Por exemplo, pretende criar um classificador para roses vs tulips. Fornecer imagens dos tulips nos campos e dos roses num vase vermelho à frente de uma firewall azul. O classificador tendo em conta estes dados, pode preparar para o campo vs lateral + vase em vez de roses vs tulips.

Para corrigir este problema, utilize a documentação de orientação na formação com mais diversificados imagens: fornecer imagens aos ângulos diferentes, os fundos dos, tamanho do objeto, grupos e outros variantes.

## <a name="negative-image-handling"></a>Processamento de imagem negativo

O serviço de visão personalizada suporta algumas processamento de imagem negativo automática. Se está a criar um cat vs classificador preguiçoso e submeter uma imagem de um shoe para predição, o classificador deve Pontuar dessa imagem como próximo de 0% para cat e preguiçoso. 

> [!WARNING]
> A abordagem automática funciona para imagens claramente negativos. Poderá não funcionar bem em casos onde as imagens negativos são apenas uma variação de imagens utilizado na formação. 
>
> Por exemplo, se tiver um husky vs classificador corgi e feed numa imagem de um Pomeranian, poderá Pontuar o Pomeranian como um Husky. Se as imagens negativos deste natureza, criar uma nova etiqueta (por exemplo, "outros") e aplicá-la para as imagens de formação negativo.

## <a name="next-steps"></a>Passos Seguintes

[Utilize a predição de API](use-prediction-api.md)