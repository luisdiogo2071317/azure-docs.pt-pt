---
title: Importar uma Function App como uma API com o portal do Azure | Microsoft Docs
description: Este tutorial mostra como utilizar a Gestão de API (APIM) para importar a Function App como uma API.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 0ee83446bb08e66c7f325bdd5585b8cc0484a74e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090933"
---
# <a name="import-a-function-app-as-an-api"></a>Importar uma Function App como uma API

Este artigo mostra como importar uma Function App como uma API. O artigo também mostra como testar a API APIM.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Importar uma Function App como uma API
> * Testar a API no portal do Azure
> * Testar a API no portal do Programador

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md)
+ Certifique-se de que existe uma Function App do Azure na sua subscrição. Para obter mais informações, veja [Criar uma Function App](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ [Criar definição OpenAPI](../azure-functions/functions-openapi-definition.md) da sua Function App do Azure

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Importar e publicar uma API de back-end

1. Selecione **APIs** em **GESTÃO DE API**.
2. Selecione **Function App** na lista **Adicionar uma nova API**.

    ![Function app](./media/import-function-app-as-api/function-app-api.png)
3. Prima **Procurar** para ver a lista de Function Apps na sua subscrição.
4. Selecione a aplicação. O APIM localiza o swagger associado à aplicação selecionada, obtém-no e importa-o. 
5. Adicione um sufixo de URL de API. O sufixo é um nome que identifica esta API específica nesta instância de APIM. Tem de ser exclusivo nesta instância de APIM.
6. Associe a API a um produto e publique-a. Neste caso, é utilizado o produto "*Unlimited*".  Se pretender que a API seja publicada e esteja disponível para programadores, adicione-a a um produto. Pode fazê-lo durante a criação da API ou defini-lo mais tarde.

    Os produtos são associações de uma ou mais APIs. Pode incluir um número de APIs e disponibilizá-las para os programadores através do portal do programador. Os programadores têm de subscrever primeiro um produto para obter acesso à API. Quando subscrevem, recebem uma chave de subscrição que é válida para qualquer API nesse produto. Se tiver criado a instância APIM, já é um administrador, pelo que tem todos os produtos subscritos por predefinição.

    Por predefinição, cada instância daAPI Management é fornecida com dois produtos de exemplo:

    * **Inicial**
    * **Ilimitado**   
7. Selecione **Criar**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Preencher chaves de Funções do Azure na Gestão de API do Azure

Se as funções do Azure importadas estiverem protegidas por chaves, a Gestão de API do Azure cria automaticamente **Valores com nome**, mas não preenche as entradas com segredos. Para cada entrada, precisa de executar os passos abaixo.  

1. Navegue para o separador **Valores com nome** na instância de Gestão de API.
2. Clique numa entrada e prima **Mostrar valor** Na barra lateral.

    ![Valores com nome](./media/import-function-app-as-api/apim-named-values.png)

3. Se o conteúdo for semelhante a *código de {Nome de Função do Azure}*, dirija-se para a Aplicação de Funções do Azure importada e navegue para a sua Função do Azure.
4. Aceda à secção **Gerir** da sua Função do Azure desejada e copiar a chave relevante, com base no método de autenticação da sua Função do Azure.

    ![Function app](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Cole a chave na caixa de texto de **Valores com nome** e clique em **Guardar**.

    ![Function app](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testar a nova API de APIM no Portal do Azure

As operações podem ser chamadas diretamente a partir do Portal do Azure, que fornece um meio cómodo para ver e testar as operações de uma API.  

1. Selecione a API que criou no passo anterior.
2. Prima o separador **Teste**.
3. Selecione uma operação.

    A página apresenta campos para os parâmetros de consulta e campos para os cabeçalhos. Um dos cabeçalhos é “Ocp-Apim-Subscription-Key”, para a chave de subscrição do produto que está associado a esta API. Se tiver criado a instância de APIM, já é um administrador, pelo que a chave é preenchida automaticamente. 
1. Prima **Enviar**.

    O back-end responde com **200 OK** e alguns dados.

## <a name="call-operation"> </a>Chamar uma operação a partir do portal do programador

Também é possível chamar operações a partir do **portal do Programador** para testar as APIs. 

1. Selecione a API que criou no passo “Importar e publicar uma API de back-end”.
2. Prima **Portal do Programador**.

    O site "portal do Programador" abre-se.
3. Selecione a **API** que criou.
4. Clique na operação que pretende testar.
5. Prima **Experimente**.
6. Prima **Enviar**.
    
    Depois de uma operação ser invocada, o portal do programador apresenta o **Estado da resposta**, os **Cabeçalhos da resposta** e qualquer **Conteúdo da resposta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Transformar e proteger a sua API](transform-api.md)