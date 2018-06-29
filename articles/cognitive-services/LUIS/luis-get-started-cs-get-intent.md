---
title: Tutorial para aprender a chamar uma aplicação Language Understanding (LUIS) com C# | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com C#.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 0416d19d27810a2ab8eeb20e16b2f921fc7826ee
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263494"
---
# <a name="tutorial-call-a-luis-endpoint-using-c"></a>Tutorial: Chamar um ponto final do LUIS com C#

Transmita expressões a um ponto final do LUIS e obtenha a intenção e as entidades.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar uma subscrição do LUIS e copiar o valor da chave para utilização posterior
> * Ver os resultados do ponto final do LUIS no browser para publicar a aplicação de IoT de exemplo
> * Criar a aplicação de consola C# do Visual Studio para efetuar a chamada HTTPS para o ponto final do LUIS

<!-- link to free account --> Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="create-luis-subscription-key"></a>Criar a chave de subscrição do LUIS
1. Primeiro tem de criar uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) no portal do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

2. Inicie sessão no portal do Azure em https://portal.azure.com. 

3. Siga os passos em [Criar Chaves de Subscrição com o Azure](./luis-how-to-azure-subscription.md) para obter uma chave.

4. Regresse ao site do [LUIS](luis-reference-regions.md). Inicie sessão com a sua conta do Azure. 

    [![](media/luis-get-started-cs-get-intent/app-list.png "Captura de ecrã da lista de aplicações")](media/luis-get-started-cs-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Compreender o que o LUIS devolve

Para compreender o que uma aplicação LUIS devolve, pode colar o URL de uma aplicação LUIS de exemplo numa janela do browser. A aplicação de exemplo é uma aplicação de IoT que deteta se o utilizador quer ligar ou desligar as luzes.

1. O ponto final da aplicação de exemplo está neste formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Copie o URL e substitua a chave de subscrição pelo valor do campo `subscription-key`.
2. Cole o URL numa janela do browser e prima Enter. O browser apresenta um resultado JSON que indica que o LUIS deteta a intenção `HomeAutomation.TurnOn` e a entidade `HomeAutomation.Room` com o valor `bedroom`.

    ![O resultado JSON deteta a intenção TurnOn](./media/luis-get-started-cs-get-intent/turn-on-bedroom.png)
3. Altere o valor do parâmetro `q=` no URL para `turn off the living room light` e prima Enter. O resultado indica agora que o LUIS detetou a intenção `HomeAutomation.TurnOff` e a entidade `HomeAutomation.Room` com o valor `living room`. 

    ![O resultado JSON deteta a intenção TurnOff](./media/luis-get-started-cs-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-c"></a>Consumir um resultado do LUIS com a API de ponto final com C# 

Pode utilizar o C# para aceder aos mesmos resultados que viu na janela do browser no passo anterior. 

1. Crie uma nova aplicação de consola no Visual Studio. Copie o código que se segue e guarde-o num ficheiro *.cs:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/endpoint-api-samples/csharp/Program.cs)]
1. Substitua o valor da variável `subscriptionKey` pela sua chave de subscrição do LUIS.

3. No projeto do Visual Studio, adicione uma referência a **System.Web**.

4. Execute a aplicação de consola. Apresenta o mesmo JSON que viu anteriormente na janela do browser.

![A janela da consola apresenta o resultado JSON do LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="clean-up-resources"></a>Limpar recursos
Os dois recursos criados neste tutorial são a chave de subscrição do LUIS e o projeto C#. Elimine a chave de subscrição do LUIS no portal do Azure. Feche o projeto do Visual Studio e remova o diretório do sistema de ficheiros. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-cs-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website