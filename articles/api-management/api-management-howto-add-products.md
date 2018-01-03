---
title: Como criar e publicar um produto na API Management do Azure
description: Saiba como criar e publicar produtos na API Management do Azure.
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
ms.openlocfilehash: e6b11145506780f9a08799c4c9daf55ba17b366d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-publish-a-product"></a>Criar e publicar um produto  

Na API Management do Azure, um produto contém uma ou mais APIs, bem como uma quota de utilização e os termos de utilização. Depois de um produto é publicado, os programadores podem subscrever o produto e começar a utilizar as APIs do produto.  

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar e publicar um produto
> * Adicionar uma API ao produto

![produto adicionado](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-and-publish-a-product"></a>Criar e publicar um produto

1. Clique em **produtos** no menu à esquerda para apresentar o **produtos** página.
2. Clique em **+ produto**.

    ![produto adicionado](media/api-management-howto-add-products/add-product.png)

    Quando adiciona um produto, terá de fornecer as seguintes informações: 

    |Nome|Descrição|
    |---|---|
    |Nome a apresentar|O nome pretende para apresentar no **portal do programador**.|
    |Nome|Um nome descritivo do produto.|
    |Descrição|O **Descrição** campo permite-lhe fornecer informações detalhadas sobre o produto como o seu objetivo, as APIs que fornece acesso ao e outras informações úteis.|
    |Estado|Prima **publicada** se pretender publicar o produto. Antes de poder ser chamadas as APIs de um produto, o produto tem de ser publicado. Por predefinição novos produtos são anular a publicação e são visíveis apenas para o **administradores** grupo.|
    |Requer aprovação|Verifique **exigir a aprovação de subscrição** se pretender que um administrador reveja e aceite ou rejeite as tentativas de subscrição para este produto. Se a caixa está desmarcada, as tentativas de subscrição são aprovadas automaticamente. |
    |Limite da contagem de subscrição|Para limitar a contagem de várias subscrições simultâneas, introduza o limite de subscrição. |
    |Termos legais|Pode incluir os termos de utilização para o produto tem de aceitar os subscritores para poder utilizar o produto.|
    |APIs|Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. <br/> Pode adicionar uma API existente durante a criação de produto. Pode adicionar uma API ao produto mais tarde, a partir de produtos **definições** página ou ao criar uma API.|<br/>Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto.<br/> Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.|

3. Clique em **criar** para criar o novo produto.

### <a name="add-more-configurations"></a>Adicionar mais configurações

Pode continuar a configurar o produto após guardá-lo ao escolher o **definições** separador. 

Ver/Adicionar subscritores do produto a partir de **subscrições** separador.

Definir uma visibilidade de um produto para os programadores ou convidados a partir de **controlo de acesso** separador.

## <a name="add-apis"></a>Adicionar APIs a um produto

Produtos são associações de APIs de um ou mais. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. Pode adicionar uma API existente durante a criação de produto. Pode adicionar uma API ao produto mais tarde, a partir de produtos **definições** página ou ao criar uma API.

Os programadores têm primeiro de subscrever um produto para obter acesso à API. Quando estes subscrever, recebem uma chave de subscrição é válida para qualquer API esse produto. Se tiver criado a instância APIM, um administrador já estiver, pelo que tem subscritos todos os produtos por predefinição.

### <a name="add-an-api-to-an-existing-product"></a>Adicionar uma API para um produto existente

1. Selecione um produto.
2. Selecione o separador APIs.
3. Clique em **+ API**.
4. Escolha uma API e clique em **criar**.

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar e publicar um produto
> * Adicionar uma API ao produto

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Criar API em branco e mock respostas de API](mock-api-responses.md)
