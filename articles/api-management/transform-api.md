---
title: Transformar e proteger a sua API com a Gestão de API do Azure | Microsoft Docs
description: Saiba como proteger a sua API com quotas e políticas de limitação (limitação de taxas).
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: bebfabfa2c9012fa55bfc6964dc0b638cb7ab3f1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936488"
---
# <a name="transform-and-protect-your-api"></a>Transformar e proteger a sua API 

O tutorial mostra como transformar a API para que a mesma não revele informações de um back-end privado. Por exemplo, poderá pretender ocultar as informações sobre a pilha de tecnologia que está em execução no back-end. Pode também querer ocultar URLs originais que aparecem no corpo da resposta HTTP da API e, em vez disso, redirecioná-los para o gateway de APIM.

Este tutorial também mostra como é fácil adicionar proteção à sua API de back-end através da configuração do limite de taxas com a Gestão de API do Azure. Por exemplo, poderá limitar um número de chamadas à API para que não seja sobreutilizada pelos programadores. Para obter mais informações, veja [Políticas de Gestão de API](api-management-policies.md)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Transformar uma API para remover os cabeçalhos de resposta
> * Substituir os URLs originais no corpo de resposta da API pelos URLs de gateway de APIM
> * Proteger uma API ao adicionar a política de limite de taxas (limitação)
> * Testar as transformações

![Políticas](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Pré-requisitos

+ Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
+ Conclua também o tutorial seguinte: [Importar e publicar a sua primeira API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Transformar uma API para remover os cabeçalhos de resposta

Esta secção mostra como ocultar os cabeçalhos HTTP que não pretende mostrar aos seus utilizadores. Neste exemplo, os seguintes cabeçalhos são eliminados na resposta HTTP:

* **X-Powered-By**
* **X-AspNet-Version**

### <a name="test-the-original-response"></a>Testar a resposta original

Para ver a resposta original:

1. Selecione o separador **API**.
2. Clique em **API da Conferência de Demonstração** na lista de API.
3. Selecione a operação **GetSpeakers**.
4. Clique no separador **Teste**, na parte superior do ecrã.
5. Prima o botão **Enviar**, na parte inferior do ecrã. 

    Como pode ver, a resposta original tem o seguinte aspeto:

    ![Políticas](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Definir a política de transformação

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Selecione **Todas as operações**.
5. Na parte superior do ecrã, selecione o separador **Design**.
6. Na janela **Processamento de saída**, clique no triângulo (junto ao lápis).
7. Selecione o **Editor de código**.
    
     ![Editar política](./media/set-edit-policies/set-edit-policies01.png)
9. Posicione o cursor no interior do elemento **&lt;saída&gt;**.
10. Na janela direita, em **Políticas de transformação**, clique em **+ Definir cabeçalho de HTTP** duas vezes (para inserir dois fragmentos de política).

    ![Políticas](./media/transform-api/transform-api.png)
11. Modifique o código **<outbound>** para ter o seguinte aspeto:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Substituir os URLs originais no corpo de resposta da API pelos URLs de gateway de APIM

Esta secção mostra como ocultar os URLs originais que aparecem no corpo da resposta HTTP da API e, em vez disso, redirecioná-los para o gateway de APIM.

### <a name="test-the-original-response"></a>Testar a resposta original

Para ver a resposta original:

1. Selecione o separador **API**.
2. Clique em **API da Conferência de Demonstração** na lista de API.
3. Selecione a operação **GetSpeakers**.
4. Clique no separador **Teste**, na parte superior do ecrã.
5. Prima o botão **Enviar**, na parte inferior do ecrã. 

    Como pode ver, a resposta original tem o seguinte aspeto:

    ![Políticas](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Definir a política de transformação

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Selecione **Todas as operações**.
5. Na parte superior do ecrã, selecione o separador **Design**.
6. Na janela **Processamento de saída**, clique no triângulo (junto ao lápis).
7. Selecione o **Editor de código**.
8. Posicione o cursor no interior do elemento **&lt;saída&gt;**.
9. Na janela direita, em **Políticas de transformação**, clique em **+ Localizar e substituir a cadeia no corpo**.
10. Modifique o código **<find-and-replace** (no elemento **<outbound>**) para substituir o URL para que corresponda ao seu gateway de APIM. Por exemplo:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Proteger uma API ao adicionar a política de limite de taxas (limitação)

Esta secção mostra como adicionar proteção à API de back-end através da configuração de limites de taxas. Por exemplo, poderá limitar um número de chamadas à API para que não seja sobreutilizada pelos programadores. Neste exemplo, o limite está definido para 3 chamadas por cada 15 segundos, para cada ID de subscrição. Após os 15 segundos, um programador pode tentar chamar a API novamente.

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Selecione **Todas as operações**.
5. Na parte superior do ecrã, selecione o separador **Design**.
6. Na janela **Processamento de entrada**, clique no triângulo (junto ao lápis).
7. Selecione o **Editor de código**.
8. Posicione o cursor no interior do elemento **&lt;entrada&gt;**.
9. Na janela direita, em **Políticas de restrição de acesso**, clique em **+ Limitar taxa de chamadas por chave**.
10. Modifique o código **<rate-limit-by-key** (no elemento **<inbound>**) para o seguinte código:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testar as transformações
        
Neste momento, as políticas de código têm o seguinte aspeto:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

O resto desta secção testa as transformações de política que definiu neste artigo.

### <a name="test-the-stripped-response-headers"></a>Testar os cabeçalhos de resposta removidos

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Clique na operação **GetSpeakers**.
5. Selecione o separador **Teste**.
6. Prima **Enviar**.

    Como pode ver, os cabeçalhos foram removidos:

    ![Políticas](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testar o URL substituído

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Clique na operação **GetSpeakers**.
5. Selecione o separador **Teste**.
6. Prima **Enviar**.

    Como pode ver, o URL foi substituído.

    ![Políticas](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testar o limite de taxa (limitação)

1. Navegue até à sua instância APIM.
2. Selecione o separador **API**.
3. Clique em **API da Conferência de Demonstração** na lista de API.
4. Clique na operação **GetSpeakers**.
5. Selecione o separador **Teste**.
6. Prima **Enviar** três vezes seguidas.

    Depois de enviar o pedido 3 vezes, obterá a resposta **429 Demasiados pedidos**.
7. Aguarde 15 segundos e prima **Enviar** novamente. Neste momento, deve obter uma resposta **200 OK**.

    ![Limitação](./media/transform-api/test-throttling.png)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Transformar uma API para remover os cabeçalhos de resposta
> * Substituir os URLs originais no corpo de resposta da API pelos URLs de gateway de APIM
> * Proteger uma API ao adicionar a política de limite de taxas (limitação)
> * Testar as transformações

Avance para o tutorial seguinte:

> [!div class="nextstepaction"]
> [Monitorizar a sua API](api-management-howto-use-azure-monitor.md)
