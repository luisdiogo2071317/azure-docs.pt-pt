---
title: Definir e utilizar fluxos de trabalho no Moderator conteúdo do Azure | Microsoft Docs
description: Saiba como criar fluxos de trabalho personalizados com base nas suas políticas de conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/07/2018
ms.author: sajagtap
ms.openlocfilehash: dfe3ba8a2ef1bcbc69ef585b504a9367d9420bf0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351596"
---
# <a name="define-test-and-use-workflows"></a>Definir, testar e utilizar fluxos de trabalho

Pode utilizar o estruturador de fluxo de trabalho do Azure conteúdo Moderator e APIs para definir os limiares com base no seu conteúdas políticas e fluxos de trabalho personalizados.

Fluxos de trabalho "ligam" para a API de Moderator conteúdo utilizando conectores. Pode utilizar outras APIs quando um conector para essa API estiver disponível. O exemplo aqui utiliza o conector de Moderator conteúdo que está incluído por predefinição.

## <a name="browse-to-the-workflows-section"></a>Navegue para a secção de fluxos de trabalho

No **definições** separador, selecione **fluxos de trabalho**.

  ![A definição de fluxos de trabalho](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Iniciar um novo fluxo de trabalho

Selecione **adicionar fluxo de trabalho**.

  ![Adicionar um fluxo de trabalho](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Atribua um nome e descrição

Nome do seu fluxo de trabalho, introduza uma descrição e escolher se o fluxo de trabalho processa imagens ou texto.

  ![Nome do fluxo de trabalho e a descrição](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Defina os critérios de avaliação ("condição")

Na captura de ecrã seguinte, pode ver os campos e as seleções de pessoa se-, em seguida, tem de definir para os fluxos de trabalho. Escolha um conector. Este exemplo utiliza **Moderator conteúdo**. Consoante o conector que escolher, alterar as opções disponíveis para saída.

  ![Definir a condição de fluxo de trabalho](images/ocr-workflow-step-2-condition.PNG)

Depois de escolher o conector e o respetivo resultado que pretende, selecione um operador e o valor para a condição.

## <a name="define-the-action-to-take"></a>Definir a ação a tomar

Selecione a ação a tomar e a condição para satisfazer. O exemplo seguinte cria uma revisão de imagem, atribui uma tag `a`e realça-lo para a condição mostrada. Também pode combinar várias condições para obter resultados que pretende. Opcionalmente, adicione um caminho alternativo de (Else).

  ![Definir ação de fluxo de trabalho](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>Guardar o fluxo de trabalho

Por fim, guarde o fluxo de trabalho e tome nota do nome de fluxo de trabalho. É necessário o nome para iniciar uma tarefa de moderação interrupção, utilizando a API de revisão.

## <a name="test-the-workflow"></a>Testar o fluxo de trabalho

Agora que definiu um fluxo de trabalho personalizado, testá-lo com conteúdo de exemplo.

Selecionar o correspondente **executar o fluxo de trabalho** botão.

  ![Teste de fluxo de trabalho](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Carregar um ficheiro

Guardar o [imagem de exemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) para disco local. Para testar o fluxo de trabalho, selecione **escolher ficheiros** e carregue a imagem.

  ![Carregar ficheiro de imagem](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Controlar o fluxo de trabalho

Controle o fluxo de trabalho, como a ser executada.

  ![Controlar a execução do fluxo de trabalho](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Reveja quaisquer imagens sinalizadas para humana moderação de interrupção

Para ver a imagem, rever, visite o **imagem** separador em **rever**.

  ![Rever imagens](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Passos Seguintes 

Para invocar o fluxo de trabalho a partir do código, utilize os fluxos de trabalho personalizados com o [ `Job` início rápido de consola API](../try-review-api-job.md) e [início rápido do .NET SDK](../moderation-jobs-quickstart-dotnet.md).
