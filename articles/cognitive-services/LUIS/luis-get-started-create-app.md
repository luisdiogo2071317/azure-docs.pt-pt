---
title: Criar a sua primeira aplicação Language Understanding (LUIS) em 10 minutos – LUIS dos Serviços Cognitivos | Microsoft Docs
description: Neste início rápido, crie uma aplicação LUIS que utiliza o domínio pré-concebido `HomeAutomation` para ligar e desligar as luzes e os eletrodomésticos. Este domínio pré-concebido fornece intenções, entidades e expressões de exemplo. Quando terminar, obterá um ponto final de LUIS em execução na cloud.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: diberry
ms.openlocfilehash: 457f23936dec0cf85e9aebbf3e54bba37c2f3ca3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "43771645"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Início Rápido: utilizar a aplicação Home Automation pré-concebida

Neste início rápido, crie uma aplicação LUIS que utiliza o domínio pré-concebido `HomeAutomation` para ligar e desligar as luzes e os eletrodomésticos. Este domínio pré-concebido fornece intenções, entidades e expressões de exemplo. Quando terminar, obterá um ponto final de LUIS em execução na cloud.

## <a name="prerequisites"></a>Pré-requisitos

Para este artigo, precisa de uma conta de LUIS gratuita criada no portal do LUIS em [http://www.luis.ai](http://www.luis.ai). 

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

Selecione **Intents** (Intenções) no painel de navegação do lado esquerdo para rever as intenções do domínio HomeAutomation. 

[![](media/luis-quickstart-new-app/home-automation-intents.png "Captura de ecrã da lista de intenções com os nomes das intenções na tabela realçados")](media/luis-quickstart-new-app/home-automation-intents.png)

Cada intenção tem expressões de exemplo.

> [!NOTE]
> **None** (Nenhuma) é uma intenção fornecida por todas as aplicações LUIS. Pode utilizá-la para processar expressões que não correspondem à funcionalidade que a sua aplicação fornece. 

Selecione a intenção **HomeAutomation.TurnOff**. Pode ver que a intenção contém uma lista de expressões que têm etiquetas de entidades.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Captura de ecrã da intenção HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-your-app"></a>Preparar a aplicação

Selecione **Train** (Preparar) no painel de navegação superior.

[![](media/luis-quickstart-new-app/trained.png "Captura de ecrã da intenção HomeAutomation.TurnOff com notificação de êxito verde")](media/luis-quickstart-new-app/trained.png)

## <a name="test-your-app"></a>Testar a aplicação
Assim que preparar a sua aplicação, pode testá-la. Selecione **Test** (Testar) no painel de navegação superior. Escreva uma expressão de teste, tal como "Turn off the lights" (Desligar as luzes) no painel Interactive Testing (Testes Interativos) e prima Enter. 

```
Turn off the lights
```

Verifique se a intenção com a melhor classificação corresponde à intenção esperada para cada expressão de teste.

Neste exemplo, "Turn off the lights" (Desligar as luzes) é identificado corretamente como a intenção com a melhor classificação de "HomeAutomation.TurnOff".

[![](media/luis-quickstart-new-app/test.png "Captura de ecrã do painel Test (Testar) com expressão realçada")](media/luis-quickstart-new-app/test.png)


Selecione **Test** (Testar) novamente para fechar o painel de teste. 

## <a name="publish-your-app"></a>Publicar a aplicação
Selecione **Publish** (Publicar) no painel de navegação superior. 

[![](media/luis-quickstart-new-app/publish.png "Captura de ecrã da aplicação com o botão Publish (Publicar) realçado")](media/luis-quickstart-new-app/publish.png)

Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

Uma barra de notificação verde na parte superior indica que a aplicação foi publicada com êxito.

[![](media/luis-quickstart-new-app/published.png "Captura de ecrã da aplicação com publicação bem-sucedida")](media/luis-quickstart-new-app/published.png)

Depois de publicar com êxito, pode utilizar o URL de ponto final apresentado na página **Publish app** (Publicar aplicação).

[![](media/luis-quickstart-new-app/endpoint.png "Captura de ecrã da página Publish (Publicar) com o URL de ponto final realçado")](media/luis-quickstart-new-app/endpoint.png)

## <a name="use-your-app"></a>Utilizar a aplicação
Pode testar o seu ponto final publicado num browser com o URL gerado. Abra este URL no browser, defina o parâmetro de URL "&q" para a consulta de teste. Por exemplo, adicione `turn off the living room light` ao final do URL e, em seguida, prima Enter. O browser apresenta a resposta JSON do ponto final de HTTP.


[![](media/luis-quickstart-new-app/turn-off-living-room.png "Captura de ecrã do browser com o resultado JSON a detetar a intenção TurnOff")](media/luis-quickstart-new-app/turn-off-living-room.png)

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o botão de reticências (***…***) à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

Pode chamar o ponto final do código:

> [!div class="nextstepaction"]
> [Chamar um ponto final de LUIS com código](luis-get-started-cs-get-intent.md)
