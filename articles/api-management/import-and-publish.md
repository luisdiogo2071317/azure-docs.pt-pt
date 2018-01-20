---
title: Importar e publicar a sua primeira API na API Management do Azure | Microsoft Docs
description: Saiba como importar e publicar a sua primeira API com a API Management.
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
ms.date: 11/15/2017
ms.author: apimpm
ms.openlocfilehash: ffe5ee95c66eee7dccd25a1afd2fe639cbc273f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="import-and-publish-your-first-api"></a>Importar e publicar a sua primeira API 

Este tutorial mostra como importar uma API de back-end "OpenAPI especificação", que reside no http://conferenceapi.azurewebsites.net?format=json. Esta API de back-end é fornecida pela Microsoft e alojado no Azure. 

Assim que a API de back-end é importado para gestão de API (APIM), a API de APIM torna-se uma fachada para a API de back-end. No momento que importar o API de back-end, a API de origem e a API de APIM são idênticos. APIM permite-lhe personalizar a fachada consoante as suas necessidades sem afetar o API de back-end. Para obter mais informações, consulte [transformar e proteger a sua API](transform-api.md). 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Importar a sua primeira API
> * A API de teste no portal do Azure
> * A API de teste no portal do Programador

![Nova API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Pré-requisitos

Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importação e publicar um API de back-end

Esta secção mostra como importar e publicar um OpenAPI especificação back-end da API.
 
1. Selecione **APIs** em **gestão de API**.
2. Selecione **OpenAPI especificação** da lista.

    ![Criar uma API](./media/api-management-get-started/create-api.png)

    Pode definir os valores de API durante a criação ou posterior, acedendo ao **definições** separador.  

    |Definição|Valor|Descrição|
    |---|---|---|
    |**Especificação de OpenAPI**|http://conferenceapi.azurewebsites.net?format=json|Referencia o serviço de implementação da API. API de gestão reencaminha os pedidos para este endereço.|
    |**Nome a apresentar**|*API de conferências de demonstração*|Se premir separador depois de introduzir o URL do serviço, APIM será preencha este campo com base no que é no json. <br/>Este nome é apresentado no portal do programador.|
    |**Nome**|*demo-conference-api*|Fornece um nome exclusivo para a API. <br/>Se premir separador depois de introduzir o URL do serviço, APIM será preencha este campo com base no que é no json.|
    |**Descrição**|Forneça uma descrição opcional da API.|Se premir separador depois de introduzir o URL do serviço, APIM será preencha este campo com base no que é no json.|
    |**Sufixo do URL da API**|*conference*|O sufixo é acrescentado para o URL de base para o serviço de gestão de API. Gestão de API distingue APIs pelo respetivo sufixo e, por conseguinte, o sufixo tem de ser exclusivo para cada API para um determinado publicador.|
    |**Esquema de URL**|*HTTPS*|Determina os protocolos podem ser utilizados para aceder à API do. |
    |**Produtos**|*Ilimitado*| Publica a API ao associar a API com um produto. Para adicionar, opcionalmente, esta nova API a um produto, escreva o nome de produto. Este passo pode ser repetido múltiplas vezes para adicionar a API a vários produtos.<br/>Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto. Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.<br/> Por predefinição, cada instância da API Management vem com dois produtos de exemplo: **Starter** e **ilimitada**. |
3. Selecione **Criar**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testar a nova API APIM no portal do Azure

Operações podem ser chamadas diretamente a partir do portal do Azure, que fornece uma maneira conveniente para ver e testar as operações de uma API.  
1. Selecione a API que criou no passo anterior.
2. Prima a **teste** separador.  ![API de teste](./media/api-management-get-started/test-api.png)
3. Clique em **GetSpeakers**.
    A página apresenta os campos para os parâmetros de consulta, mas neste caso, não temos qualquer. A página também apresenta os campos para os cabeçalhos. Um dos cabeçalhos de é "Ocp-Apim-Subscription-Key", para a chave de subscrição do produto que está associado esta API. A chave é automaticamente preenchida.
4. Prima **Enviar**.

    Back-end responde com **200 OK** e alguns dados.

## <a name="call-operation"> </a>Chamar uma operação a partir do portal do programador

Também podem ser chamadas operações **portal do programador** para testar as APIs. 

1. Selecione a API que criou no "Import e publicar uma API de back-end" passo.
2. Prima **portal do programador**.

    ![Teste no portal do Programador](./media/api-management-get-started/developer-portal.png)

    O site "Portal do programador" abre-se.
3. Selecione **API**.
4. Selecione **demonstração conferência API**.
5. Clique em **GetSpeakers**.
    
    A página apresenta os campos para os parâmetros de consulta, mas neste caso, não temos qualquer. A página também apresenta os campos para os cabeçalhos. Um dos cabeçalhos de é "Ocp-Apim-Subscription-Key", para a chave de subscrição do produto que está associado esta API. Se tiver criado a instância APIM, um administrador já estiver, pelo que a chave é automaticamente preenchida.
6. Prima **experimente**.
7. Prima **Enviar**.
    
    Depois de uma operação ser invocada, o portal do programador apresenta o **Estado da resposta**, os **Cabeçalhos da resposta** e qualquer **Conteúdo da resposta**.

## <a name="next-steps"> </a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Importar a sua primeira API
> * A API de teste no portal do Azure
> * A API de teste no portal do Programador

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Criar e publicar um produto](api-management-howto-add-products.md)
