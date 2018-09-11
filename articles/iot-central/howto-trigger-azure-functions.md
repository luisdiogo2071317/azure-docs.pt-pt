---
title: Funções do Azure de Acionador com webhooks no Azure IoT Central
description: Crie uma aplicação de função que será executada sempre que é acionada uma regra no Azure IoT Central.
author: viv-liu
ms.author: viviali
ms.date: 09/06/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 7b14dc4eeec1ab543c407b1bb1cf8a5ce46f3ecb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356909"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Funções do Azure de Acionador com webhooks no Azure IoT Central

Utilize as funções do Azure para executar código sem servidor o webhook de saída do Centro de IoT regras. Não precisa de aprovisionar uma VM ou publicar uma aplicação web para utilizar as funções do Azure, mas em vez disso, pode executar esse código serverlessly. Utilize as funções do Azure para transformar o payload do webook antes de os enviar para o destino final, como uma base de dados SQL ou o Event Grid. 

## <a name="prerequisites"></a>Pré-requisitos
+ Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="how-to-connect-azure-functions"></a>Como ligar as funções do Azure

1. [Crie uma nova aplicação de função no Portal do Azure. ](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).
2. Expanda a aplicação de função e clique na + junto a funções. Se esta função for a primeira na sua aplicação function app, selecione a função personalizada. É apresentado o conjunto completo de modelos de função.
3. No campo de pesquisa, escreva genérico e, em seguida, escolha o idioma pretendido para o modelo de Acionador de webhook genérico. Este tópico utiliza uma função C#. 
4. Na função nova, clique em **</> Obter URL da função** e, em seguida, copie e guarde o valor. Utilize este valor para configurar o webhook. 
4. No Centro de IoT, encontre a regra que pretende ligar à sua aplicação de função. 
5. Adicione uma ação do webhook. Introduza um **nome a apresentar** e cole o URL de função que copiou anteriormente.
6. Guarde a regra. Agora quando a regra é acionada, o webhook invocará a aplicação de funções para executar. Na sua aplicação de função, pode observar os registos e ver sempre que a função é acionada.

Para obter mais informações, visite o artigo de funções do Azure sobre [criar uma função acionada por um webhook genérico](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu a configurar e utilizar webhooks, a próxima etapa sugerida é explorar [criação de fluxos de trabalho no Microsoft Flow](howto-add-microsoft-flow.md).
