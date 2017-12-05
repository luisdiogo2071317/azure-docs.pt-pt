---
title: Adicionar uma API manualmente no portal do Azure | Microsoft Docs
description: "Este tutorial mostra como utilizar a gestão de API (APIM) para adicionar manualmente uma API."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 9426839f88daece1bb688a2079b7854ccaebdc57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="add-an-api-manually"></a>Adicionar manualmente uma API 

Os passos neste artigo mostram como utilizar o portal do Azure para adicionar uma API manualmente a instância de gestão de API (APIM). É um cenário comum quando pretender criar uma API em branco e defini-lo manualmente quando pretender mock a API. Para obter detalhes sobre mocking uma API, consulte [respostas de Mock API](mock-api-responses.md).

Se pretender importar uma API existente, consulte [relacionadas com tópicos](#related-topics) secção.

Neste artigo, vamos criar uma API em branco e especificar [httpbin.org](http://httpbin.org) (um serviço de teste) como uma API de back-end.

## <a name="prerequisites"></a>Pré-requisitos

Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Criar uma API

1. Selecione **APIs** em **gestão de API**.
2. No menu à esquerda, selecione **+ adicionar API**.
3. Selecione **API em branco** da lista.

    ![API em branco](media/add-api-manually/blank-api.png)
4. Introduza as definições para a API.

    ![Definições](media/add-api-manually/settings.png)

    |**Nome**|**Valor**|**Descrição**|
    |---|---|---|
    |**Nome a apresentar**|"*Em branco API*" |Este nome é apresentado no portal do programador.|
    |**URL do serviço Web** (opcional)| "*http://httpbin.org*"| Se pretender mock uma API, não poderá introduzir nada. <br/>Neste caso, iremos introduzir [http://httpbin.org](http://httpbin.org). Este é um serviço de teste público. <br/>Se pretender importar uma API que está mapeada para um back-end automaticamente, consulte um dos tópicos no [relacionadas com tópicos](#related-topics) secção.|
    |**Esquema de URL**|"*HTTPS*"|Neste caso, apesar do back-end tem acesso HTTP não seguro, especificamos um acesso seguro de HTTPS APIM para o back-end. <br/>Este tipo de cenário (HTTPS para HTTP) é chamado a terminação HTTPS. Poderá fazê-lo se a sua API existe dentro de uma rede virtual (em que sabe que o acesso é seguro, mesmo se não for utilizado a HTTPS). <br/>Pode querer utilizar "Terminação HTTPS" para guardar algumas ciclos da CPU.|
    |**Sufixo do URL**|"*hbin*"| O sufixo é um nome que identifique nesta API específica nesta instância APIM. Tem de ser exclusivo nesta instância APIM.|
    |**Produtos**|"*Ilimitados*" |Publica a API ao associar a API com um produto. Se pretender que para a API ser publicado e estar disponível para programadores, adicione-a um produto. Pode fazê-lo durante a criação de API ou defini-lo mais tarde.<br/><br/>Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. <br/>Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto. Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.<br/><br/> Por predefinição, cada instância da API Management vem com dois produtos de exemplo: **Starter** e **ilimitada**.| 
5. Selecione **Criar**.

Neste momento, não tem nenhum operações em APIM que mapeiam para as operações na sua API de back-end. Se chamar uma operação que está exposta através do back-end, mas não através de APIM, obter um **404**. 

>[!NOTE] 
> Por predefinição, quando adicionar uma API, mesmo que está ligada a algumas serviço de back-end, APIM não exporá quaisquer operações até whitelist-los. A lista branca de uma operação do seu serviço de back-end, criar uma operação de APIM que mapeia para a operação de back-end.
>

## <a name="add-and-test-an-operation"></a>Adicionar e testar uma operação

Esta secção mostra como adicionar uma operação "/ obter" para mapeá-lo para a operação de "http://httpbin.org/get" back-end.

### <a name="add-the-operation"></a>Adicionar a operação

1. Selecione a API que criou no passo anterior.
2. Clique em **+ adicionar operação**.
3. No **URL**, selecione **obter** e introduza "*/obter*" no recurso.
4. Introduza "*FetchData*" para **nome a apresentar**.
5. Selecione **Guardar**.

### <a name="test-the-operation"></a>A operação de teste

A operação de teste no portal do Azure. Em alternativa, pode testar-na **portal do programador**.

1. Selecione o **teste** separador.
2. Selecione **FetchData**.
3. Prima **enviar**.

É apresentada a resposta que a operação de "http://httpbin.org/get" gera. Se pretender as operações de transformação, consulte [transformar e proteger a sua API](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Adicionar e testar uma operação com parâmetros

Esta secção mostra como adicionar uma operação que assume um parâmetro. Neste caso, iremos mapear a operação "http://httpbin.org/status/200".

### <a name="add-the-operation"></a>Adicionar a operação

1. Selecione a API que criou no passo anterior.
2. Clique em **+ adicionar operação**.
3. No **URL**, selecione **obter** e introduza "*/status/ {code}*" no recurso. Opcionalmente, pode fornecer algumas informações associadas a este parâmetro. Por exemplo, introduza "*número*" para **tipo**, "*200*" (predefinição) para **valores**.
4. Introduza "GetStatus" para **nome a apresentar**.
5. Selecione **Guardar**.

### <a name="test-the-operation"></a>A operação de teste 

A operação de teste no portal do Azure.  Em alternativa, pode testar-na **portal do programador**.

1. Selecione o **teste** separador.
2. Selecione **GetStatus**. Por predefinição, o valor de código está definido como "*200*". Pode alterar para outros valores de teste. Por exemplo, escreva "*418*".
3. Prima **enviar**.

    É apresentada a resposta que a operação de "http://httpbin.org/status/200" gera. Se pretender as operações de transformação, consulte [transformar e proteger a sua API](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Transformar e proteger uma API publicada](transform-api.md)
