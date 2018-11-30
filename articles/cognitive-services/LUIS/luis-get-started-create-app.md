---
title: 10 minutos para a sua primeira aplicação LUIS
titleSuffix: Azure Cognitive Services
description: Crie uma aplicação LUIS que utilize o domínio pré-concebido `HomeAutomation` para ligar e desligar as luzes e os eletrodomésticos. Este domínio pré-concebido fornece intenções, entidades e expressões de exemplo. Quando terminar, obterá um ponto final de LUIS em execução na cloud.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 657f5b6879f7782cbd94588657dc0082eff1f9c5
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423335"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Início Rápido: utilizar a aplicação Home Automation pré-concebida

Neste início rápido, crie uma aplicação LUIS que utiliza o domínio pré-concebido `HomeAutomation` para ligar e desligar as luzes e os eletrodomésticos. Este domínio pré-concebido fornece intenções, entidades e expressões de exemplo. Quando terminar, obterá um ponto final de LUIS em execução na cloud.

## <a name="prerequisites"></a>Pré-requisitos

Para este artigo, precisa de uma conta de LUIS gratuita criada no portal do LUIS em [https://www.luis.ai](https://www.luis.ai). 

## <a name="create-a-new-app"></a>Criar uma nova aplicação
Pode criar e gerir as suas aplicações em **My Apps** (As Minhas Aplicações). 

1. Inicie sessão no portal do LUIS.

2. Selecione **Create new app** (Criar nova aplicação).

    [![](media/luis-quickstart-new-app/app-list.png "Captura de ecrã da lista de aplicações")](media/luis-quickstart-new-app/app-list.png)

3. Na caixa de diálogo, atribua o nome "Home Automation" à aplicação.

    [![](media/luis-quickstart-new-app/create-new-app-dialog.png "Captura de ecrã da caixa de diálogo de pop-up Criar nova aplicação")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Escolha a cultura da sua aplicação. Para esta aplicação Home Automation, selecione English (Inglês). Em seguida, selecione **Done** (Concluído). O LUIS cria a aplicação Home Automation. 

    >[!NOTE]
    >A cultura não pode ser alterada assim que a aplicação for criada. 

## <a name="add-prebuilt-domain"></a>Adicionar um domínio pré-concebido

Selecione **Prebuilt domains** (Domínios pré-concebidos) no painel de navegação do lado esquerdo. Em seguida, procure "Home". Selecione **Add domain** (Adicionar domínio).

[![](media/luis-quickstart-new-app/home-automation.png "Captura de ecrã do domínio HomeAutomation no menu Prebuilt domains (Domínios pré-concebidos)")](media/luis-quickstart-new-app/home-automation.png)

Quando o domínio for adicionado com êxito, a caixa Prebuilt domains (Domínios pré-concebidos) apresenta um botão **Remove domain** (Remover domínio).

[![](media/luis-quickstart-new-app/remove-domain.png "Captura de ecrã do domínio HomeAutomation com o botão Remove (Remover)")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Intenções e entidades

Selecione **Intents** (Intenções) no painel de navegação do lado esquerdo para rever as intenções do domínio HomeAutomation. Cada intenção tem expressões de exemplo.

> [!NOTE]
> **None** (Nenhuma) é uma intenção fornecida por todas as aplicações LUIS. Pode utilizá-la para processar expressões que não correspondem à funcionalidade que a sua aplicação fornece. 

Selecione a intenção **HomeAutomation.TurnOff**. Pode ver que a intenção contém uma lista de expressões que têm etiquetas de entidades.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Captura de ecrã da intenção HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Testar a aplicação
Assim que preparar a sua aplicação, pode testá-la. Selecione **Test** (Testar) no painel de navegação superior. Escreva uma expressão de teste, tal como "Turn off the lights" (Desligar as luzes) no painel Interactive Testing (Testes Interativos) e prima Enter. 

```
Turn off the lights
```

Verifique se a intenção com a melhor classificação corresponde à intenção esperada para cada expressão de teste.

Neste exemplo, "Turn off the lights" (Desligar as luzes) é identificado corretamente como a intenção com a melhor classificação de "HomeAutomation.TurnOff".

[![](media/luis-quickstart-new-app/test.png "Captura de ecrã do painel Test (Testar) com expressão realçada")](media/luis-quickstart-new-app/test.png)


Selecione **Test** (Testar) novamente para fechar o painel de teste. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vá para o final do URL no endereço, introduza `turn off the living room light` e prima Enter. O browser apresenta a resposta JSON do ponto final de HTTP.

    [![](media/luis-quickstart-new-app/turn-off-living-room.png "Captura de ecrã do browser com o resultado JSON a detetar a intenção TurnOff")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

Pode chamar o ponto final do código:

> [!div class="nextstepaction"]
> [Chamar um ponto final de LUIS com código](luis-get-started-cs-get-intent.md)
