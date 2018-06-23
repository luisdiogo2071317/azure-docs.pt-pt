---
title: Testar e reparametrização de um serviços cognitivos do modelo - Service de visão personalizada - Azure | Microsoft Docs
description: Saiba como uma imagem de teste e, em seguida, utilizá-lo para a reparametrização do modelo.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 1933b1a45844ac99308baebe59b49687a957abfa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353281"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Testar e reparametrização de um modelo com o serviço de visão personalizada

Depois de preparar o seu modelo, pode testar rapidamente a-la utilizando uma imagem armazenada localmente ou uma imagem online. O teste utiliza a iteração treinada mais recentemente.

## <a name="test-your-model"></a>Testar o seu modelo

1. Do [página web de visão personalizada](https://customvision.ai), selecione o seu projeto. Selecione **teste rápida** à direita da barra de menu superior. Esta ação abre uma janela de etiqueta **teste rápida**.

    ![O botão de teste rápido é apresentado no canto superior direito da janela.](./media/test-your-model/quick-test-button.png)

2. No **testar rápida** janela, clique no **Submeter imagem** campo e introduza o URL da imagem que pretende utilizar para o teste. Se pretender utilizar uma imagem de armazenada localmente em vez disso, clique em de **procurar ficheiros locais** botão e seleccione um ficheiro de imagem local.

    ![Imagem da página de imagem de envio](./media/test-your-model/submit-image.png)

A imagem que selecionou é apresentado no meio da página. Em seguida, os resultados são apresentados abaixo a imagem no formato de uma tabela com colunas de duas, com a etiqueta **etiquetas** e **confiança**. Depois de ver os resultados, pode fechar o **teste rápida** janela.

Pode agora adicionar esta imagem de teste para o seu modelo e, em seguida, reparametrização do seu modelo.

## <a name="use-the-predicted-image-for-training"></a>Utilize a imagem prevista para formação.

Para utilizar a imagem submetida anteriormente para formação, utilize os seguintes passos:

1. Para ver imagens submetidas para o classificador, abra o [página web de visão personalizada](https://customvision.ai) e selecione o __Predições__ separador.

    ![Imagem de separador predições](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > A vista predefinida mostra imagens a partir da iteração atual. Pode utilizar o __iteração__ pendente de campo para ver imagens submetidas durante iterações anteriores.

2. Coloque o cursor sobre uma imagem para ver as etiquetas que foram rigor o classificador.

    > [!TIP]
    > Imagens estão ordenadas, para que as imagens que podem colocar os maioria dos ganhos o classificador estão na parte superior. Para selecionar uma ordenação diferente, utilize o __ordenação__ secção.

    Para adicionar uma imagem aos seus dados de formação, selecione a imagem, selecione a etiqueta e, em seguida, selecione __guarde e feche__. A imagem é removida do __Predições__ e adicionados às imagens de formação. Pode vê-la, selecionando o __imagens de formação__ separador.

    ![Imagem da página etiquetagem](./media/test-your-model/tag-image.png)

3. Utilize o __formação__ botão para a reparametrização do classificador.

## <a name="next-steps"></a>Passos Seguintes

[Melhorar a sua classificador](getting-started-improving-your-classifier.md)