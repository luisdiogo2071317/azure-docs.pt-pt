---
title: Importar uma API de SOAP e converter em REST utilizando o portal do Azure | Microsoft Docs
description: "Saiba como importar uma API de SOAP e convertê-lo para a REST API Management."
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
ms.openlocfilehash: 74935f11d5bf3c83aef46c1d41fccd81ad312acb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="import-a-soap-api-and-convert-to-rest"></a>Importar uma API de SOAP e converter em REST

Este artigo mostra como importar uma API de SOAP e REST convertê-la. O artigo também mostra como a API de APIM de teste.

Neste artigo, saiba como:

> [!div class="checklist"]
> * Importar uma API de SOAP e converter em REST
> * A API de teste no portal do Azure
> * A API de teste no portal do Programador

## <a name="prerequisites"></a>Pré-requisitos

Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importação e publicar uma API de back-end

1. Selecione **APIs** em **gestão de API**.
2. Selecione **WSDL** do **adicionar uma nova API** lista.

    ![API DE SOAP](./media/restify-soap-api/wsdl-api.png)
3. No **WSDL specification**, introduza o URL para onde reside a API de SOAP.
4. Clique em **SOAP resto** botão de opção. Quando esta opção é clicada, APIM tenta efetuar uma transformação automática entre XML e JSON. Neste caso, os consumidores devem chamar a API como uma API restful, que devolve JSON. APIM é conversão de cada pedido para uma chamada SOAP.

    ![SOAP para REST](./media/restify-soap-api/soap-to-rest.png)

5. Separador prima.

    Os campos seguintes obterem preenchidos cópias de segurança com as informações a partir da API de SOAP: nome a apresentar, nome, descrição.
6. Adicione um sufixo do URL da API. O sufixo é um nome que identifique nesta API específica nesta instância APIM. Tem de ser exclusivo nesta instância APIM.
9. Publica a API ao associar a API com um produto. Neste caso, o "*ilimitada*" produto é utilizado.  Se pretender que para a API ser publicado e estar disponível para programadores, adicione-a um produto. Pode fazê-lo durante a criação de API ou defini-lo mais tarde.

    Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto. Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.

    Por predefinição, cada instância daAPI Management é fornecida com dois produtos de exemplo:

    * **Inicial**
    * **Ilimitado**   
10. Selecione **Criar**.

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