---
title: Depurar as suas APIs utilizando o rastreio de pedidos na API Management do Azure | Microsoft Docs
description: Siga os passos deste tutorial para saber como inspecionar os passos na API Management do Azure de processamento de pedidos.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 7b9bec7927169b9d820c095a7d11705264e7dcfe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="debug-your-apis-using-request-tracing"></a>Depurar as suas APIs utilizando o rastreio de pedidos

Este tutorial descreve como inspecionar o processamento de pedidos para ajudá-lo com a API de resolução de problemas e depuração. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Uma chamada de rastreio

![Inspector de API](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="trace-a-call"></a>Uma chamada de rastreio

1. Selecione **APIs**.
2. Clique em **demonstração conferência API** na sua lista de API.
3. Selecione **GetSpeakers** operação.
4. Mudar para o **teste** separador.
5. Certifique-se de que inclui um cabeçalho de HTTP chamado **Ocp-Apim-rastreio** com o valor definido para **verdadeiro**.
6. Clique em **"Enviar"** para efetuar uma chamada de API. 
7. Aguarde a chamada concluir. 
8. Vá para o **rastreio** separador o **consola API**. Pode clicar em qualquer uma das ligações seguintes para ir para as informações de rastreio de detalhado: **entrada**, **back-end**, **saída**.

    No **entrada** secção, pode ver o pedido original API Management recebeu o autor da chamada e todas as políticas aplicadas ao pedido, incluindo as políticas de cabeçalho de conjunto de limite de taxa e foi adicionado no passo 2.

    No **back-end** secção, pode ver os pedidos de API Management enviado para o back-end de API e a resposta que recebeu.
    
    No **saída** secção, pode ver todas as políticas aplicadas à resposta antes de enviar novamente para o autor da chamada.
 
    > [!TIP]
    > Cada passo também mostra o tempo decorrido desde que o pedido é recebido através da API Management.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Uma chamada de rastreio

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Utilize as revisões](api-management-get-started-revise-api.md)
