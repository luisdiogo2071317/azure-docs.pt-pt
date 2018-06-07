---
title: Ver e eliminar os dados a partir da galeria do Azure AI | Microsoft Docs
description: Pode exportar e eliminar os dados de utilizador no produto a partir da Galeria de AI do Azure utilizando a interface ou a API do catálogo de AI da galeria. Este artigo mostra-lhe como.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: bc6ffa912d7914c8662dbde623e04947540ac149
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655614"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Ver e eliminar dados de utilizador no produto da Galeria de AI do Azure

Pode ver e eliminar os dados de utilizador no produto a partir da Galeria de AI do Azure utilizando a interface ou a API do catálogo de AI da galeria. Este artigo mostra-lhe como.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>Ver os seus dados na Galeria de AI com a IU

Pode ver itens publicadas através do Web site da galeria do Azure AI IU. Os utilizadores podem ver soluções públicas e não listadas, projetos, experimentações e outros itens publicados:

1.  Iniciar sessão para o [galeria do Azure AI](https://gallery.azure.ai/).
2.  Clique na imagem do perfil no canto superior direito e, em seguida, o nome da conta para carregar a página do seu perfil.
3.  A página perfil apresenta todos os itens publicados para a galeria, incluindo entradas não listadas.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>Utilize a API do catálogo de AI Galeria para ver os seus dados

Pode ver através de programação dados recolhidos através de API do catálogo de AI galeria, que é acessível na https://catalog.cortanaanalytics.com/entities. Para ver os dados, terá do ID do autor. Para ver as entidades não listadas através da API do catálogo, precisa de um token de acesso.

Catálogo respostas são devolvidas em formato JSON.

### <a name="get-an-author-id"></a>Obter um ID do autor
O ID do autor baseia-se no endereço de e-mail utilizado quando publicar Galeria de AI do Azure. Não altera:

1.  Inicie sessão no [galeria do Azure AI](https://gallery.azure.ai/).
2.  Clique na imagem do perfil no canto superior direito e, em seguida, o nome da conta para carregar a página do seu perfil.
3.  O URL na barra de endereço apresenta o seguinte ID alfanumérico `authorId=`. Por exemplo, para o URL: `https://gallery.cortanaintelligence.com/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    ID do autor: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Obter o token de acesso

Precisa de um token de acesso para ver as entidades não listadas através da API do catálogo. Sem Token um acesso, os utilizadores podem continuar a ver entidades públicas e outras informações de utilizador.

Para obter um token de acesso, terá de inspecionar o `DataLabAccessToken` cabeçalho de um pedido HTTP faz com que o browser para a API do catálogo enquanto tem sessão iniciada:

1.  Iniciar sessão para o [galeria do Azure AI](https://gallery.azure.ai/).
2.  Clique na imagem do perfil no canto superior direito e, em seguida, o nome da conta para carregar a página do seu perfil.
3.  Abra o painel de ferramentas de programador do browser, premindo F12, selecione o separador de rede e atualize a página. 
4. Filtrar pedidos a cadeia de *catálogo* escrevendo na caixa de texto de filtro.
5.  Nos pedidos para o URL `https://catalog.cortanaanalytics.com/entities`, encontrar um pedido GET e selecione o *cabeçalhos* separador. Desloque para baixo até o *cabeçalhos de pedido* secção.
6.  Sob o cabeçalho `DataLabAccessToken` é o token alfanumérico. Para ajudar a manter os seus dados seguros, não partilhe este token.

### <a name="view-user-information"></a>Ver informações de utilizador
Com o ID do autor recebeu nos passos anteriores, ver informações do perfil do utilizador, substituindo `[AuthorId]` no URL seguinte:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Por exemplo, este pedido de URL:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Devolve uma resposta, tais como:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Vista de entidades públicas

A API do catálogo armazena informações sobre entidades publicadas para a Galeria de AI do Azure que também pode ver diretamente no [Web site da Galeria de AI](https://gallery.azure.ai/). 

Para ver as entidades publicadas, visite o seguinte URL, substituindo `[AuthorId]` com o ID do autor obtido no [obter uma ID de autor](#get-an-author-ID) acima.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Por exemplo:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Ver entidades públicas e não listadas

Esta consulta apresenta apenas as entidades públicas. Para ver todas as entidades, incluindo aqueles não listados, forneça o acesso ao obter o token da secção anterior.

1.  Utilizando uma ferramenta como [Postman](https://www.getpostman.com), crie um pedido de HTTP GET para o URL do catálogo, conforme descrito em [obter o token de acesso](#get-your-access-token).
2.  Criar um cabeçalho de pedido HTTP chamado `DataLabAccessToken`, com o valor definido para o token de acesso.
3.  Submeta o pedido HTTP.

> [!TIP]
> Se não listadas entidades são não ser apresentado nas respostas a partir da API do catálogo, o utilizador pode ter um inválido ou expirado token de acesso. Termine sessão Galeria de AI do Azure e, em seguida, repita os passos [obter o token de acesso](#get-your-access-token) para renovar o token. 
