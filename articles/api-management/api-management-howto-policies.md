---
title: "As políticas de API Management do Azure | Microsoft Docs"
description: "Saiba como criar, editar e configurar políticas na API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: 8576d590568a24df4c3320ec283edba7a28ab032
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="policies-in-azure-api-management"></a>Políticas de API Management do Azure

Na gestão de API do Azure (APIM), as políticas são uma funcionalidade poderosa do sistema que permite ao publicador alterar o comportamento da API através da configuração. As políticas são uma coleção de instruções que são executadas sequencialmente no pedido ou resposta de uma API. As instruções populares incluem a conversão do formato de XML para JSON e limitação para restringir a quantidade de chamadas recebidas de um programador de taxa de chamadas. Muitas mais políticas estão disponíveis a box.

As políticas são aplicadas no interior do gateway que se encontra entre os consumidores de API e a API gerida. O gateway recebe todos os pedidos e normalmente reencaminha-os será alterado para a API subjacente. No entanto uma política pode aplicar as alterações para o pedido de entrada e de resposta de saída.

As expressões de política podem ser utilizadas como valores de atributo ou valores de texto em qualquer uma das políticas de API Management, a menos que a política especifique o contrário. Algumas políticas, como o [controlar o fluxo] [ Control flow] e [definir variável] [ Set variable] políticas baseadas em expressões de política. Para obter mais informações, consulte [políticas avançadas] [ Advanced policies] e [expressões de política][Policy expressions].

## <a name="sections"></a>Configuração da política de compreender

A definição de política é um simple um documento XML que descreve uma sequência de declarações de entrada e saídas. O XML pode ser editado diretamente na janela de definição. É fornecida uma lista de instruções para a direita e instruções aplicáveis ao âmbito atual estiverem ativadas e realçadas.

Clicar numa instrução ativada irá adicionar o XML adequado na localização de cursor na vista de definição. 

> [!NOTE]
> Se a política que pretende adicionar não estiver ativada, certifique-se de que está no âmbito correto para essa política. Cada declaração de política foi concebida para utilização num determinado âmbitos e as secções de política. Para rever as secções de política e os âmbitos de uma política, verifique o **utilização** secção para essa política no [referência de política][Policy Reference].
> 
> 

A configuração está dividida em `inbound`, `backend`, `outbound`, e `on-error`. A série de declarações de política especificado é executa para que um pedido e resposta.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Se ocorrer um erro durante o processamento de um pedido, os restantes passos no `inbound`, `backend`, ou `outbound` secções são ignoradas e execução salta para as instruções no `on-error` secção. Colocando declarações de política no `on-error` secção pode rever o erro, utilizando o `context.LastError` propriedade, Inspecione e personalizar a resposta de erro utilizando o `set-body` política e configurar o que acontece se ocorrer um erro. Existem códigos de erro para obter os passos incorporados e de erros que possam ocorrer durante o processamento de declarações de política. Para obter mais informações, consulte [erros nas políticas de gestão de API](https://msdn.microsoft.com/library/azure/mt629506.aspx).

## <a name="scopes"></a>Como configurar políticas

Para obter informações sobre como configurar políticas, consulte [definir ou editar políticas](set-edit-policies.md).

## <a name="policy-reference"></a>Referência de política

Consulte o [referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e as respetivas definições.

## <a name="policy-samples"></a>Exemplos de política

Consulte [amostras de política](policy-samples.md) para obter mais exemplos de código.

## <a name="examples"></a>Exemplos

### <a name="apply-policies-specified-at-different-scopes"></a>Aplicar políticas especificadas em âmbitos diferentes

Se tiver uma política ao nível global e uma política configurada para uma API, em seguida, sempre que essa API específica é utilizada ambas as políticas serão aplicadas. Gestão de API permite a ordenação determinista de declarações de política combinado através do elemento base. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

Na definição de política de exemplo acima, o `cross-domain` instrução iria executar antes de ser seguido de qualquer políticas superiores que seriam por sua vez, o `find-and-replace` política. 

### <a name="restrict-incoming-requests"></a>Restrinja os pedidos recebidos

Para adicionar uma nova declaração para restringir os pedidos recebidos para endereços IP especificados, coloque o cursor apenas dentro do conteúdo do `inbound` elemento XML e clique em de **restringir chamador IPs** instrução.

![Políticas de restrição][policies-restrict]

Isto irá adicionar um fragmento XML para o `inbound` elemento que fornece orientações sobre como configurar a instrução.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Para limitar os pedidos de entrada e aceitar apenas os partir de um endereço IP de 1.2.3.4 modificam o XML da seguinte forma:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações para trabalhar com as políticas, consulte:

+ [APIs de transformação](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e as respetivas definições
+ [Exemplos de política](policy-samples.md)   

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
