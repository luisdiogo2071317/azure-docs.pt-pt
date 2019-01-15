---
title: Definir e utilizar fluxos de trabalho de moderação de conteúdos - Content Moderator
titlesuffix: Azure Cognitive Services
description: Pode utilizar as APIs do designer de fluxo de trabalho do Azure Content Moderator e definir fluxos de trabalho personalizados e limiares com base em suas diretivas de conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 265d5a2bd6a256d287e6ae164e883ade783f960f
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262409"
---
# <a name="define-test-and-use-workflows"></a>Definir, testar e utilizar fluxos de trabalho

Pode utilizar as APIs do designer de fluxo de trabalho do Azure Content Moderator e definir fluxos de trabalho personalizados e limiares com base em suas diretivas de conteúdo.

Fluxos de trabalho "ligar" para a API Content Moderator utilizando conectores. Se está disponível um conector para essa API, pode usar outras APIs. Este exemplo usa o conector do Content Moderator que está incluído por predefinição.

## <a name="browse-to-the-workflows-section"></a>Navegue para a secção de fluxos de trabalho

Sobre o **configurações** separador, selecione **fluxos de trabalho**.

  ![Definição de fluxos de trabalho](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Iniciar um novo fluxo de trabalho

Selecione **adicionar fluxo de trabalho**.

  ![Adicionar um fluxo de trabalho](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Atribuir um nome e descrição

Nomeie o seu fluxo de trabalho, introduza uma descrição e escolha se o fluxo de trabalho processa imagens ou texto.

  ![Nome do fluxo de trabalho e uma descrição](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Definir os critérios de avaliação ("condição")

Captura de ecrã seguinte, verá os campos e as seleções If-Then-Else, que é necessário definir para os fluxos de trabalho. Escolha um conector. Este exemplo utiliza **Content Moderator**. Consoante o conector que escolher, alterar as opções disponíveis para a saída.

  ![Definir a condição de fluxo de trabalho](images/ocr-workflow-step-2-condition.PNG)

Depois de escolher o conector e o respetivo resultado que pretende, selecione um operador e valor para a condição.

## <a name="define-the-action-to-take"></a>Definir a ação a tomar

Selecione a ação a tomar e a condição com satisfazer. O exemplo seguinte cria uma revisão de imagem, atribui uma etiqueta `a`e destaca-lo para a condição mostrada. Também pode combinar várias condições para obter os resultados desejados. Opcionalmente, adicione um caminho alternativo de (Else).

  ![Definir ação de fluxo de trabalho](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>Guardar o fluxo de trabalho

Por fim, guarde o fluxo de trabalho e tome nota do nome do fluxo de trabalho. É necessário o nome para iniciar uma tarefa de moderação ao utilizar a API de revisão.

## <a name="test-the-workflow"></a>Testar o fluxo de trabalho

Agora que tiver definido um fluxo de trabalho personalizado, testá-la com conteúdo de exemplo.

Selecione o correspondente **executar o fluxo de trabalho** botão.

  ![Teste de fluxo de trabalho](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Carregar um ficheiro

Guardar a [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) sua unidade local. Para testar o fluxo de trabalho, selecione **Escolher ficheiro ou ficheiros** e carregar a imagem.

  ![Carregar ficheiro de imagem](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Controlar o fluxo de trabalho

Controle o fluxo de trabalho de execução.

  ![Controlar a execução de fluxo de trabalho](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Reveja quaisquer imagens sinalizadas para moderação humana

Para ver a imagem, rever, vá para o **imagem** separador sob **rever**.

  ![Rever imagens](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Passos Seguintes 

Para invocar o fluxo de trabalho a partir do código, utilize fluxos de trabalho personalizados com o [ `Job` guia de introdução de consola de API](../try-review-api-job.md) e o [início rápido do SDK de .NET](../moderation-jobs-quickstart-dotnet.md).
