---
title: Início Rápido para aprender a chamar uma aplicação Language Understanding (LUIS) com Python | Microsoft Docs
description: Neste início rápido, vai aprender a chamar uma aplicação LUIS com Python.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: bc7ae912d762a98c34b9a1b2d6a82d5630c4794b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771753"
---
# <a name="quickstart-call-a-luis-endpoint-using-python"></a>Início Rápido: chamar um ponto final de LUIS com Python
Neste início rápido, transmita expressões a um ponto final de LUIS e obtenha a intenção e as entidades.

<!-- green checkmark -->
<!--
> [!div class="checklist"]
> * Create LUIS subscription and copy key value for later use
> * View LUIS endpoint results from browser to public sample IoT app
> * Create Visual Studio C# console app to make HTTPS call to LUIS endpoint
-->

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

<a name="create-luis-subscription-key"></a>
## <a name="create-luis-endpoint-key"></a>Criar uma chave de ponto final de LUIS
Precisa de uma chave de API dos Serviços Cognitivos para efetuar chamadas para a aplicação LUIS de exemplo utilizada nestas instruções. 

Para obter uma chave de API, siga estes passos: 

1. Primeiro tem de criar uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) no portal do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

2. Inicie sessão no portal do Azure em https://portal.azure.com. 

3. Siga os passos em [Criar Chaves de Ponto Final com o Azure](./luis-how-to-azure-subscription.md) para obter uma chave.

4. Regresse ao site do [LUIS](luis-reference-regions.md) e inicie sessão com a sua conta do Azure. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Captura de ecrã da lista de aplicações")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Compreender o que o LUIS devolve

Para compreender o que uma aplicação LUIS devolve, pode colar o URL de uma aplicação LUIS de exemplo numa janela do browser. A aplicação de exemplo é uma aplicação de IoT que deteta se o utilizador quer ligar ou desligar as luzes.

1. O ponto final da aplicação de exemplo está neste formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Copie o URL e substitua a chave de ponto final pelo valor do campo `subscription-key`.
2. Cole o URL numa janela do browser e prima Enter. O browser apresenta um resultado JSON que indica que o LUIS deteta a intenção `HomeAutomation.TurnOn` e a entidade `HomeAutomation.Room` com o valor `bedroom`.

    ![O resultado JSON deteta a intenção TurnOn](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. Altere o valor do parâmetro `q=` no URL para `turn off the living room light` e prima Enter. O resultado indica agora que o LUIS detetou a intenção `HomeAutomation.TurnOff` e a entidade `HomeAutomation.Room` com o valor `living room`. 

    ![O resultado JSON deteta a intenção TurnOff](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>Consumir um resultado do LUIS com a API de ponto final com Python

Pode utilizar o Python para aceder aos mesmos resultados que viu na janela do browser no passo anterior.

1. Copie um dos seguintes fragmentos de código para um ficheiro chamado `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Substitua o valor do campo `Ocp-Apim-Subscription-Key` pela sua chave de ponto final de LUIS.

3. Instale dependências com `pip install requests`.

4. Execute o script com `python ./quickstart-call-endpoint.py`. Apresenta o mesmo JSON que viu anteriormente na janela do browser.
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>Limpar recursos
Os dois recursos criados neste tutorial são a chave de ponto final de LUIS e o projeto C#. Elimine a chave de ponto final de LUIS do portal do Azure. Feche o projeto do Visual Studio e remova o diretório do sistema de ficheiros. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-python-add-utterance.md)