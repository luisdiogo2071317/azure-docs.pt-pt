---
title: Importar uma aplicação das Funções do Azure como uma API no portal do Azure | Microsoft Docs
description: Este tutorial mostra como utilizar a Gestão de API do Azure para importar uma aplicação das Funções do Azure como uma API.
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
ms.openlocfilehash: 670fa58de7155028b0f72f1f819b9f269e07b9eb
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239057"
---
# <a name="import-an-azure-functions-app-as-an-api"></a>Importar uma aplicação das Funções do Azure como uma API

Este artigo mostra como importar uma aplicação das Funções do Azure como uma API. O artigo também mostra como testar a API de Gestão de API do Azure.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Importar uma aplicação de Funções como uma API
> * Testar a API no portal do Azure
> * Testar a API no portal do programador

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o início rápido [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
+ Certifique-se de que tem uma aplicação de Funções do Azure na sua subscrição. Para obter mais informações, veja [Criar uma aplicação de Funções](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ [Criar uma definição OpenAPI](../azure-functions/functions-openapi-definition.md) da sua aplicação de Funções do Azure.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importar e publicar uma API de back-end

1. Em **GESTÃO DE API**, selecione **APIs**.
2. Na lista **Adicionar uma nova API**, selecione **Aplicação de Funções**.

    ![Aplicação de Funções](./media/import-function-app-as-api/function-app-api.png)
3. Selecione **Procurar** para ver a lista de aplicações de Funções na sua subscrição.
4. Selecione a aplicação. A Gestão de API localiza o swagger que está associado à aplicação selecionada, obtém-no e importa-o. 
5. Adicione um sufixo de URL de API. O sufixo é um nome que identifica esta API específica nesta instância de Gestão de API. O sufixo tem de ser exclusivo nesta instância de Gestão de API.
6. Associe a API a um produto e publique-a. Neste caso, é utilizado o produto **Ilimitado**. Se pretender que a API seja publicada e esteja disponível para programadores, adicione a API a um produto. Pode adicionar a API a um produto ao criá-la ou pode adicioná-la mais tarde.

    Os produtos são associações de uma ou mais APIs. Pode incluir várias APIs e disponibilizá-las para os programadores através do portal do programador. Os programadores têm de subscrever primeiro um produto para obter acesso à API. Quando um programador subscreve, este recebe uma chave de subscrição que é válida para qualquer API nesse produto. Se tiver criado a instância de Gestão de API, é um administrador. Os administradores estão subscritos em todos os produtos por predefinição.

    Por predefinição, cada instância daAPI Management é fornecida com dois produtos de exemplo:

    * **Inicial**
    * **Ilimitado**   
7. Selecione **Criar**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Preencher chaves de Funções do Azure na Gestão de API do Azure

Se as aplicações das Funções do Azure importadas estiverem protegidas por chaves, a Gestão de API cria automaticamente *valores com nome* para as chaves. A Gestão de API não preenche as entradas com segredos. Siga os seguintes passos para cada entrada:  

1. Na instância de Gestão de API, selecione o separador **Valores com nome**.
2. Selecione uma entrada e, em seguida, selecione **Mostrar valor** na barra lateral.

    ![Valores com nome](./media/import-function-app-as-api/apim-named-values.png)

3. Se o texto apresentado na caixa **Valor** for semelhante ao código **para o \<nome das Funções do Azure\>**, aceda às **Aplicações de Funções** e, em seguida, aceda às **Funções**.
4. Selecione **Gerir** e, em seguida, copie a chave relevante com base no método de autenticação da sua aplicação.

    ![Aplicação de Funções - Copiar chaves](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Cole a chave na caixa **Valor** e, em seguida, selecione **Guardar**.

    ![Aplicação de funções - Colar valores de chave](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-api-management-api-in-the-azure-portal"></a>Testar a nova API de Gestão de API no Portal do Azure

Pode chamar operações diretamente a partir do portal do Azure. Utilizar o portal do Azure é um meio cómodo de ver e testar as operações de uma API.  

1. Selecione a API que criou na secção anterior.
2. Selecione o separador **Teste**.
3. Selecione uma operação.

    A página apresenta campos para os parâmetros de consulta e campos para os cabeçalhos. Um dos cabeçalhos é **Ocp-Apim-Subscription-Key**, para a chave de subscrição do produto que está associado a esta API. Se tiver criado a instância de Gestão de API, já é um administrador, pelo que a chave é preenchida automaticamente. 
4. Selecione **Enviar**.

    O back-end responde com **200 OK** e alguns dados.

## <a name="call-operation"></a>Chamar uma operação a partir do portal do programador

Também pode chamar operações do portal do programador para testar as APIs. 

1. Selecione a API que criou em [Importar e publicar uma API de back-end](#create-api).
2. Selecione **Portal de programador**.

    O site do portal do Programador abre-se.
3. Selecione a **API** que criou.
4. Selecione a operação que pretende testar.
5. Selecione **Experimentar**.
6. Selecione **Enviar**.
    
    Depois de uma operação ser invocada, o portal do programador apresenta o **Estado da resposta**, os **Cabeçalhos da resposta** e qualquer **Conteúdo da resposta**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Transformar e proteger a sua API](transform-api.md)