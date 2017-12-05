---
title: "Importar uma aplicação API como uma API com o portal do Azure | Microsoft Docs"
description: "Este tutorial mostra como utilizar a gestão de API (APIM) para importar aplicação API, como uma API."
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
ms.openlocfilehash: d0e1aa6763d96b5a84bbc5fba7dcae690c051eb3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-api-app-as-an-api"></a>Importar uma aplicação API como uma API

Este artigo mostra como importar uma aplicação API como uma API. O artigo também mostra como a API de APIM de teste.

Neste artigo, saiba como:

> [!div class="checklist"]
> * Importar uma aplicação API como uma API
> * A API de teste no portal do Azure
> * A API de teste no portal do Programador

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md)
+ Certifique-se de que existe uma aplicação API na sua subscrição. Para obter mais informações, consulte [Service a documentação da aplicação] [https://docs.microsoft.com/azure/app-service/]

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importação e publicar uma API de back-end

1. Selecione **APIs** em **gestão de API**.
2. Selecione **aplicação API** do **adicionar uma nova API** lista.

    ! (Aplicação de API) [. / media/import-api-app-as-api/api-app.png]
3. Prima **procurar** para ver a lista das API Apps na sua subscrição.
4. Selecione a aplicação. APIM localiza swagger associado à aplicação selecionada, obtém-lo e importa-os. 

    No caso de APIM não localizar swagger, expõe a API como uma API de "pass-through". 
5. Adicione um sufixo do URL da API. O sufixo é um nome que identifique nesta API específica nesta instância APIM. Tem de ser exclusivo nesta instância APIM.
6. Publica a API ao associar a API com um produto. Neste caso, o "*ilimitada*" produto é utilizado.  Se pretender que para a API ser publicado e estar disponível para programadores, adicione-a um produto. Pode fazê-lo durante a criação de API ou defini-lo mais tarde.

    Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto. Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.

    Por predefinição, cada instância daAPI Management é fornecida com dois produtos de exemplo:

    * **Inicial**
    * **Ilimitado**   
7. Selecione **Criar**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testar a nova API APIM no portal do Azure

Operações podem ser chamadas diretamente a partir do portal do Azure, que fornece uma maneira conveniente para ver e testar as operações de uma API.  

1. Selecione a API que criou no passo anterior.
2. Prima a **teste** separador.
3. Selecione algumas operações.

    A página apresenta os campos para os cabeçalhos e os campos para os parâmetros de consulta. Um dos cabeçalhos de é "Ocp-Apim-Subscription-Key", para a chave de subscrição do produto que está associado esta API. Se tiver criado a instância APIM, um administrador já estiver, pelo que a chave é automaticamente preenchida. 
1. Prima **enviar**.

    Back-end responde com **200 OK** e alguns dados.

## <a name="call-operation"> </a>Chamar uma operação a partir do portal do programador

Também podem ser chamadas operações **portal do programador** para testar as APIs. 

1. Selecione a API que criou no "Import e publicar uma API de back-end" passo.
2. Prima **portal do programador**.

    O site "Portal do programador" abre-se.
3. Selecione o **API** que criou.
4. Clique a que pretende testar.
5. Prima **experimente**.
6. Prima **enviar**.
    
    Depois de uma operação ser invocada, o portal do programador apresenta o **Estado da resposta**, os **Cabeçalhos da resposta** e qualquer **Conteúdo da resposta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Transformar e proteger uma API publicada](transform-api.md)