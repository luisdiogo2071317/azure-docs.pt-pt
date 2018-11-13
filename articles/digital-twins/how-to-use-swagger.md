---
title: Compreender como utilizar o Swagger do Azure Digital duplos | Documentos da Microsoft
description: Utilize o Swagger duplos Digital do Azure
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: adgera
ms.openlocfilehash: 0fae059db53668da2d71906bbe94e9dabae931bc
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578591"
---
# <a name="use-azure-digital-twins-swagger"></a>Utilize o Swagger duplos Digital do Azure

Cada instância de duplos Digital do Azure aprovisionada inclui a sua própria documentação de referência do Swagger gerada automaticamente.

[Swagger](https://swagger.io/), ou [OpenAPI](https://www.openapis.org/), une as informações de API complexas num recurso de referência interativa e de linguagem desconhecida. Swagger fornece o material de referência essencial sobre qual conteúdo JSON, métodos HTTP e pontos de extremidade específicos a utilizar para efetuar operações de uma API.

> [!IMPORTANT]
> Suporte para a autenticação de Swagger está temporariamente desativado durante a pré-visualização pública.

## <a name="swagger-summary"></a>Resumo do swagger

Swagger fornece um resumo interativo da sua API, que inclui:

* Informações de modelo de API e o objeto.
* Pontos finais da REST API que especificam o payloads de pedido necessária, cabeçalhos, parâmetros, caminhos de contexto e métodos HTTP.
* Teste de funcionalidades de API.
* Informações de resposta de exemplo utilizadas para validar e confirmar as respostas HTTP.
* Informações de código de erro.

Swagger é uma ferramenta conveniente para ajudá-lo em desenvolvimento e testes chamadas feitas para a API de gestão.

> [!TIP]
> Pré-estreia do Swagger é fornecido demonstrar o recurso de API definido.
> Ele é hospedado pelo [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Pode acessar sua própria documentação Swagger da API de gestão gerada em:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Nome personalizado | Substituir |
| --- | --- |
| yourInstanceName | O nome da sua instância de duplos Digital do Azure |
| yourLocation | Qual sua instância estiver alojada num servidor a região |

## <a name="reference-material"></a>Material de referência

Material de referência gerada automaticamente explica conceitos fundamentais e modelos de objeto.

Um resumo conciso descreve a API.

![Parte superior do swagger][1]

Modelos de objeto de API Core também estão listados.

![Modelos de swagger][2]

Pode selecionar cada modelo de objeto listadas para obter um resumo mais detalhado de atributos de chaves.

![Modelo de swagger][3]

Os modelos de objeto do Swagger gerados são convenientes ver todos os gémeos de Digital do Azure disponíveis [objetos e APIs](./concepts-objectmodel-spatialgraph.md). Os desenvolvedores podem fazer utilizar deste recurso quando eles criam soluções no duplos Digital do Azure.

## <a name="endpoint-summary"></a>Ponto final de resumo

Swagger também fornece uma visão geral completa de todos os pontos finais que compõem a API.

Cada ponto de extremidade listado também inclui as informações de solicitação necessários, como o:

* Parâmetros necessários.
* Tipos de dados de parâmetro necessário.
* Método HTTP para aceder ao recurso.

![Pontos de extremidade do swagger][4]

Para ver uma descrição mais detalhada, selecione cada recurso.

## <a name="use-swagger-to-test-endpoints"></a>Utilize o Swagger para pontos finais de teste

É uma das funcionalidades poderosas que swagger fornece a capacidade de testar um ponto final de API diretamente através da documentação da interface do Usuário.

Depois de selecionar um ponto de extremidade específico, consulte **experimentá-lo**.

![Tente do swagger][5]

Expanda essa secção para aparecer em campos de entrada para cada um dos parâmetros obrigatório e opcional. Introduza os valores em conformidade e selecione **Execute**.

![Tentei do swagger][6]

Depois de executar o teste, pode validar os dados de resposta.

## <a name="swagger-response-data"></a>Dados de resposta do swagger

Cada ponto de extremidade listado também inclui dados de corpo de resposta para validar o seu desenvolvimento e testes. Estes exemplos incluem os códigos de estado e o JSON que pretende ver os pedidos HTTP com êxito.

![Resposta do swagger][7]

Os exemplos incluem também os códigos de erro para o ajudar a depurar ou melhorar a falhar de testes.

## <a name="swagger-oauth-20-authorization"></a>Autorização de OAuth 2.0 de swagger

Para saber mais sobre testar interativamente os pedidos protegidos pelo OAuth 2.0, consulte a [documentação oficial](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> Suporte para a autenticação do OAuth 2.0 está temporariamente desativado durante a pré-visualização pública.

## <a name="next-steps"></a>Passos Seguintes

Leia mais sobre modelos de objetos duplos Digital do Azure e o gráfico de inteligência espaciais, leia [modelos de objeto de compreender duplos de Digital do Azure](./concepts-objectmodel-spatialgraph.md).

Para saber como autenticar com a API de gestão, leia [autenticar com as APIs](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
