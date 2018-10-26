---
title: Noções básicas sobre como utilizar o Swagger do Azure Digital duplos | Documentos da Microsoft
description: Como utilizar o Swagger do Azure Digital duplos
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 3bc365c204ab75a2f136c3e26c4b598b25f66114
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093494"
---
# <a name="how-to-use-azure-digital-twins-swagger"></a>Como utilizar o Swagger do Azure Digital duplos

Cada instância de duplos Digital do Azure aprovisionada inclui a sua própria documentação de referência do Swagger gerada automaticamente.

[Swagger](https://swagger.io/) (ou [OpenAPI](https://www.openapis.org/)) une as informações de API complexas num recurso de referência interativa e de linguagem desconhecida. Especificamente, o Swagger fornece material de referência essencial sobre qual conteúdo JSON, métodos HTTP e pontos de extremidade específicos a utilizar para efetuar operações de uma API.

## <a name="swagger-summary"></a>Resumo do swagger

Swagger fornece um resumo interativo da sua API, incluindo:

* Informações de API e o modelo de objeto.
* Pontos finais da API de REST especificando necessário payloads de pedido, cabeçalhos, parâmetros, caminhos de contexto e métodos HTTP.
* Teste de funcionalidades de API.
* Informações de resposta de exemplo para validar e confirmar as respostas HTTP.
* Informações de código de erro.

Swagger, portanto, é uma ferramenta conveniente para ajudá-lo em desenvolvimento e testes chamadas feitas para a API de gestão.

> [!TIP]
> Para referência, pré-estreia do Swagger é fornecido demonstrar o recurso de API definido.
> Ele é hospedado pelo [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Pode acessar seu próprios, documentação gerada, Swagger da API de gestão em:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourInstanceName* | O nome da sua instância de duplos Digital do Azure |
| *yourLocation* | Qual sua instância estiver alojada num servidor a região |

## <a name="reference-material"></a>Material de referência

Material de referência gerada automaticamente explica conceitos fundamentais e modelos de objeto.

Um resumo conciso descreve a API:

![Parte superior do swagger][1]

Modelos de objeto de API Core também estão listados:

![Modelos de swagger][2]

Pode clicar em cada modelo de objeto listadas para obter um resumo mais detalhado de atributos de chaves:

![Modelo de swagger][3]

Os modelos de objeto Swagger gerado são convenientes ver todos os gémeos de Digital do Azure disponíveis [objetos e APIs](./concepts-objectmodel-spatialgraph.md). É um ótimo recurso para os desenvolvedores usem ao criar soluções em duplos Digital do Azure.

## <a name="endpoint-summary"></a>Ponto final de resumo

Swagger também fornece uma visão geral completa de todos os pontos finais que compõem a API.

Cada ponto de extremidade listado também inclui as informações de solicitação necessários, tais como:

* Parâmetros necessários.
* Tipos de dados de parâmetro necessário.
* O método HTTP para aceder ao recurso.

![Pontos de extremidade do swagger][4]

Cada recurso pode ser clicado para ver uma descrição mais detalhada.

## <a name="using-swagger-to-test-endpoints"></a>Com o Swagger para testar os pontos finais

É uma das funcionalidades poderosas Swagger fornece a capacidade de **experimentá-lo** ou um ponto final de API diretamente através da documentação da interface do Usuário de teste.

Depois de clicar num ponto final específico, verá uma **experimentá-lo** botão:

![Tente do swagger][5]

Expandir essa secção abre campos de entrada para cada um dos parâmetros obrigatório e opcional. Introduza os valores em conformidade e clique em **Execute**:

![Tentei do swagger][6]

Depois de executar o teste, pode validar os dados de resposta.

## <a name="swagger-response-data"></a>Dados de resposta do swagger

Cada ponto de extremidade listado também inclui dados de corpo de resposta para validar o seu desenvolvimento e testes. Estes exemplos incluem os códigos de estado pretendido e JSON para pedidos HTTP com êxito.

![Resposta do swagger][7]

Os exemplos incluem também os códigos de erro para o ajudar a depurar ou melhorar a falhar de testes.

## <a name="swagger-oauth-20-authorization"></a>Autorização de OAuth 2.0 de swagger

Para testar interativamente a pedidos relativos a recursos da API protegidos pelo OAuth 2.0, consulte a [documentação oficial](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Passos Seguintes

Para ler mais sobre modelos de objetos duplos Digital do Azure e o gráfico de inteligência espaciais, leia [este artigo](./concepts-objectmodel-spatialgraph.md).

Para saber como autenticar com a API de gestão, leia [autenticar com as APIs](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
