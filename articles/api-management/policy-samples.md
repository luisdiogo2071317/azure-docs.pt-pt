---
title: "Exemplos de políticas de API Management do Azure | Microsoft Docs"
description: "Saiba mais sobre as políticas disponíveis para utilização na API Management do Azure."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: sample
ms.date: 10/31/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: e951eb47290a2d1b6f892f1ccf923f6ce0c1899c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="api-management-policy-samples"></a>Exemplos de política de gestão de API

[Políticas](api-management-howto-policies.md) são uma funcionalidade poderosa do sistema que permite ao publicador alterar o comportamento da API através da configuração. As políticas são uma coleção de instruções que são executadas sequencialmente no pedido ou na resposta de uma API. A tabela seguinte inclui ligações para exemplos e fornece uma descrição breve de cada amostra.

|||
|---|---|
|**Políticas de entrada**||
|[Adicionar um cabeçalho de Forwarded para permitir que a API para construir o URL correto de back-end](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) |Demonstra como adicionar um cabeçalho de Forwarded do pedido de entrada para permitir que o back-end da API para construir o URL correto.|
|[Adicionar um cabeçalho que contém um id de correlação](./policies/add-correlation-id.md?toc=api-management/toc.json) |Demonstra como adicionar um cabeçalho que contém um ID de correlação para o pedido de entrada.|
|[Adicionar capacidades para um serviço de back-end e a resposta em cache](./policies/cache-response.md?toc=api-management/toc.json) |Mostra como adicionar capacidades para um serviço de back-end. Por exemplo, aceite um nome do lugar em vez de latitude e longitude numa previsão de Meteorologia API.|
|[Autorizar o acesso com base em afirmações JWT](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json) |Mostra como autorizar o acesso aos métodos HTTP específicos numa API baseadas em afirmações JWT.|
|[Autorizar o acesso a utilizar o token OAuth do Google](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json) |Mostra como autorizar o acesso aos pontos finais da sua utilização Google como um fornecedor de tokens de OAuth.|
|[Gerar a assinatura de acesso partilhado e reencaminhar pedido para o armazenamento do Azure](./policies/generate-shared-access-signature.md?toc=api-management/toc.json) |Mostra como gerar [assinatura de acesso partilhado](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) utilizando expressões e reencaminhá-los o pedido para o armazenamento do Azure com a política de reescrever-uri. |
|[Obtenha o acesso de OAuth2 token do AAD e reencaminhá-lo para o back-end](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json) |Fornece e exemplo de como utilizar o OAuth2 para autorização entre o gateway e um back-end. Mostra como obter um token de acesso do AAD e reencaminhá-lo para o back-end.|
|[Token de obter X-CSRF do gateway SAP através da política de pedido de envio](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json) |Mostra como implementar o padrão de X CSRF utilizado por muitas APIs. Neste exemplo é específico ao SAP Gateway. |
|[O pedido com base no tamanho do respetivo corpo de rotas](./policies/route-requests-based-on-size.md?toc=api-management/toc.json) |Demonstra como encaminhar pedidos com base no tamanho do respetivos corpos.|
|[Enviar informações de contexto de pedido para o serviço de back-end](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json) |Mostra como enviar algumas informações de contexto para o serviço de back-end para o registo ou processamento.|
|[Definir a duração da cache de resposta](./policies/set-cache-duration.md?toc=api-management/toc.json) |Demonstra como definir a duração da cache de resposta utilizando o valor de maxAge no cabeçalho de Cache-Control enviado pelo back-end.|
|**Políticas de saída**||
|[Filtrar o conteúdo da resposta](./policies/filter-response-content.md?toc=api-management/toc.json) | Demonstra como elementos de dados do payload de resposta com base no produto associado ao pedido de filtro.|
|**Políticas no erro**||
|[Registar erros no Stackify](./policies/log-errors-to-stackify.md?toc=api-management/toc.json) |Mostra como adicionar uma política de registo de erro a enviar erros ao Stackify para registo.|
