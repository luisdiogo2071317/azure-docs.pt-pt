---
title: Transformar e proteger a sua API com a API Management do Azure | Microsoft Docs
description: "Saiba como proteger a sua API com quotas e políticas de limitação (limitação de taxas)."
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
ms.openlocfilehash: 772f3828d85c54e7b8bb44c857e555175b7444cc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="transform-and-protect-your-api"></a>Transformar e proteger a sua API 

O tutorial mostra como a API de transformação para revelar a informação de back-end privada. Por exemplo, poderá pretender ocultar as informações sobre a pilha de tecnologia que está em execução no back-end. Pode também querer ocultar URLs originais que aparecem no corpo da resposta HTTP da API e, em vez disso redirecioná-los para o gateway APIM.

Este tutorial também mostra como é fácil adicionar proteção para a API de back-end ao configurar o limite de velocidade API Management do Azure. Por exemplo, poderá limitar um número de chamadas que de API é chamado pelo que não está a ser overused pelos programadores. Para obter mais informações, consulte [políticas de gestão de API](api-management-policies.md)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Transformar um API para eliminar os cabeçalhos de resposta
> * Substitua o URL original no corpo de resposta da API APIM URLs de gateway
> * Proteger uma API ao adicionar a política de limite de velocidade (limitação)
> * Testar as transformações

![Políticas](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Transformar um API para eliminar os cabeçalhos de resposta

Esta secção mostra como ocultar os cabeçalhos HTTP que não pretende mostrar os seus utilizadores. Neste exemplo, os seguintes cabeçalhos obterem eliminados na resposta HTTP:

* **X-se ligados à corrente-por**
* **Versão de AspNet X**

### <a name="test-the-original-response"></a>A resposta original de teste

Para ver a resposta original:

1. Selecione o **API** separador.
2. Clique em **demonstração conferência API** na sua lista de API.
3. Selecione o **GetSpeakers** operação.
4. Clique em de **teste** separador, na parte superior do ecrã.
5. Prima a **enviar** , na parte inferior do ecrã. 

    Como pode ver que a resposta original tem o seguinte aspeto:

    ![Políticas](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Definir a política de transformação

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Selecione **todas as operações**.
5. Na parte superior do ecrã, selecione **Design** separador.
6. No **processamento saído** janela, clique em triângulo (junto a lápis).
7. Selecione **editor de código**.
    
     ![Editar política](./media/set-edit-policies/set-edit-policies01.png)
9. Posicione o cursor no interior do  **<outbound>**  elemento.
10. Na janela da direita, em **políticas de transformação**, clique em **+ cabeçalho de HTTP de conjunto** duas vezes (para inserir dois fragmentos de política).

    ![Políticas](./media/transform-api/transform-api.png)
11. Modificar o  **<outbound>**  código para ter o seguinte aspeto:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Substitua o URL original no corpo de resposta da API APIM URLs de gateway

Esta secção mostra como ocultar URLs originais que aparecem no corpo da resposta HTTP da API e, em vez disso redirecioná-los para o gateway APIM.

### <a name="test-the-original-response"></a>A resposta original de teste

Para ver a resposta original:

1. Selecione o **API** separador.
2. Clique em **demonstração conferência API** na sua lista de API.
3. Selecione o **GetSpeakers** operação.
4. Clique em de **teste** separador, na parte superior do ecrã.
5. Prima a **enviar** , na parte inferior do ecrã. 

    Como pode ver que a resposta original tem o seguinte aspeto:

    ![Políticas](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Definir a política de transformação

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Selecione **todas as operações**.
5. Na parte superior do ecrã, selecione **Design** separador.
6. No **processamento saído** janela, clique em triângulo (junto a lápis).
7. Selecione **editor de código**.
8. Posicione o cursor no interior do  **<outbound>**  elemento.
9. Na janela da direita, em **políticas de transformação**, clique em **+ localizar e substituir a cadeia no corpo**.
10. Modificar o **< localizar e substituir** código (no  **<outbound>**  elemento) para substituir o URL para corresponder ao seu gateway APIM. Por exemplo:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Proteger uma API ao adicionar a política de limite de velocidade (limitação)

Esta secção mostra como adicionar proteção para a API de back-end através da configuração de limites de velocidade. Por exemplo, poderá limitar um número de chamadas que de API é chamado pelo que não está a ser overused pelos programadores. Neste exemplo, o limite está definido para 3 chamadas por 15 segundos para cada ID de subscrição Após 15 segundos, um programador pode tentar novamente a chamar a API de mensagens em fila.

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Selecione **todas as operações**.
5. Na parte superior do ecrã, selecione **Design** separador.
6. No **processamento entrado** janela, clique em triângulo (junto a lápis).
7. Selecione **editor de código**.
8. Posicione o cursor no interior do  **<inbound>**  elemento.
9. Na janela da direita, em **políticas de restrição de acesso**, clique em **+ limitar taxa de chamadas por chave**.
10. Modificar o **< taxa limite por chave** código (no  **<inbound>**  elemento) para o seguinte código:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testar as transformações
        
Neste momento as políticas de código parece que este:

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

As restantes desta secção testes transformações de política que definir neste artigo.

### <a name="test-the-stripped-response-headers"></a>Testar os cabeçalhos de resposta repartidas

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Clique em de **GetSpeakers** operação.
5. Selecione o **teste** separador.
6. Prima **enviar**.

    Como pode ver que os cabeçalhos tem sido eliminados:

    ![Políticas](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testar o URL substituído

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Clique em de **GetSpeakers** operação.
5. Selecione o **teste** separador.
6. Prima **enviar**.

    Como pode ver o URL foi substituído.

    ![Políticas](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testar o limite de velocidade (limitação)

1. Navegue até à sua instância APIM.
2. Selecione o **API** separador.
3. Clique em **demonstração conferência API** na sua lista de API.
4. Clique em de **GetSpeakers** operação.
5. Selecione o **teste** separador.
6. Prima **enviar** três vezes numa linha.

    Depois de enviar o pedido 3 vezes, obterá **429 demasiados pedidos muitos** resposta.
7. Aguarde 15 segundos ou e prima **enviar** novamente. Neste momento, pode ser obtido um **200 OK** resposta.

    ![Limitação](./media/transform-api/test-throttling.png)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Transformar um API para eliminar os cabeçalhos de resposta
> * Substitua o URL original no corpo de resposta da API APIM URLs de gateway
> * Proteger uma API ao adicionar a política de limite de velocidade (limitação)
> * Testar as transformações

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Monitorizar a sua API](api-management-howto-use-azure-monitor.md)
