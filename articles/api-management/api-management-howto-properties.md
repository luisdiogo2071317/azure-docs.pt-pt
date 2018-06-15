---
title: Como utilizar propriedades nas políticas de API Management do Azure
description: Saiba como utilizar propriedades nas políticas de API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: apimpm
ms.openlocfilehash: e0559380f6d686a4e559779c4271ea85106558d6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197117"
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Como utilizar propriedades nas políticas de API Management do Azure
Políticas de API Management são uma funcionalidade poderosa do sistema que permitem que o portal do Azure alterar o comportamento da API através da configuração. As políticas são uma coleção de instruções que são executadas sequencialmente no pedido ou na resposta de uma API. Declarações de política podem ser construídas com valores de texto literal, expressões de política e as propriedades. 

Cada instância de serviço de API Management tem uma coleção de propriedades de pares chave/valor que são globais para a instância de serviço. Estas propriedades podem ser utilizadas para gerir os valores de cadeia constante em todas as políticas de configuração da API e. Cada propriedade pode ter os seguintes atributos:

| Atributo | Tipo | Descrição |
| --- | --- | --- |
| Nome a apresentar |cadeia |Cadeia alfanumérica utilizada para referenciar a propriedade nas políticas. |
| Valor |cadeia |O valor da propriedade. Não pode estar vazios ou consistir apenas em espaço em branco. |
|Segredo|boolean|Determina se o valor é um segredo e deve ser encriptado ou não.|
| Etiquetas |Matriz da cadeia |Opcional de etiquetas que quando é fornecido pode ser utilizado para filtrar a lista de propriedades. |

![Valores com nome](./media/api-management-howto-properties/named-values.png)

Os valores de propriedade podem conter cadeias literais e [expressões de política](https://msdn.microsoft.com/library/azure/dn910913.aspx). Por exemplo, o valor de `ExpressionProperty` é uma expressão de política que devolve uma cadeia contendo a data e hora atuais. A propriedade `ContosoHeaderValue` está marcada como um segredo, pelo que não é apresentado o respetivo valor.

| Nome | Valor | Segredo | Etiquetas |
| --- | --- | --- | --- |
| ContosoHeader |TrackingId |Falso |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |Verdadeiro |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |Falso | |

## <a name="to-add-and-edit-a-property"></a>Para adicionar e editar uma propriedade

![Adicionar uma propriedade](./media/api-management-howto-properties/add-property.png)

1. Selecione **APIs** em **gestão de API**.
2. Selecione **denominado valores**.
3. Prima **+ adicionar**.

  Nome e valor são os valores necessários. Se este valor de propriedade é um segredo, verifique a esta é uma caixa de verificação secreta. Introduza um ou mais etiquetas opcionais para ajudar a organizar as propriedades e clique em Guardar.
4. Clique em **Criar**.

Quando a propriedade for criada, pode editá-lo ao clicar na propriedade. Se alterar o nome da propriedade, as políticas que referenciem essa propriedade são atualizadas automaticamente para utilizar o novo nome.

Para informações sobre como editar uma propriedade utilizando a API REST, consulte [editar uma propriedade utilizando a API REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Para eliminar uma propriedade

Para eliminar uma propriedade, clique em **eliminar** junto a propriedade para eliminar.

> [!IMPORTANT]
> Se a propriedade é referenciada pelas políticas, será possível eliminá-lo com êxito até que remova a propriedade de todas as políticas que a utilizam.
> 
> 

Para obter informações sobre como eliminar uma propriedade utilizando a API REST, consulte [eliminar uma propriedade utilizando a API REST](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Para procurar e filtrar as propriedades

O **denominado valores** separador inclui a procura e filtragem de funcionalidades para o ajudar a gerir as propriedades. Para filtrar a lista de propriedades por nome de propriedade, introduza um termo de pesquisa no **Procurar propriedade** caixa de texto. Para apresentar todas as propriedades, desmarque a **Procurar propriedade** caixa de texto e prima introduza.

Para filtrar a lista de propriedades por valores de etiqueta, introduza uma ou mais etiquetas para o **filtrar por etiquetas** caixa de texto. Para apresentar todas as propriedades, desmarque a **filtrar por etiquetas** caixa de texto e prima introduza.

## <a name="to-use-a-property"></a>Para utilizar uma propriedade

Para utilizar uma propriedade de uma política, coloque o nome da propriedade dentro de um par duplo de chavetas como `{{ContosoHeader}}`, conforme mostrado no exemplo seguinte:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

Neste exemplo, `ContosoHeader` é utilizado como o nome de um cabeçalho num `set-header` política, e `ContosoHeaderValue` é utilizado como o valor de que o cabeçalho. Quando esta política é avaliada durante um pedido ou resposta para o gateway de gestão de API, `{{ContosoHeader}}` e `{{ContosoHeaderValue}}` são substituídos com os respetivos valores de propriedade correspondentes.

Propriedades podem ser utilizadas como atributo concluído ou valores de elemento, conforme mostrado no exemplo anterior, mas também podem ser inseridas ou combinados com parte de uma expressão de texto literal, conforme mostrado no exemplo seguinte:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Propriedades também podem conter expressões de política. No exemplo seguinte, o `ExpressionProperty` é utilizado.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Quando esta política é avaliada, `{{ExpressionProperty}}` é substituído com o respetivo valor: `@(DateTime.Now.ToString())`. Uma vez que o valor de uma expressão de política, a expressão é avaliada e a política continua com a execução.

Pode testar horizontalmente no portal do programador ao chamar uma operação que tem uma política com as propriedades do âmbito. No exemplo seguinte, uma operação é chamada com o exemplo anterior dois `set-header` políticas com propriedades. Tenha em atenção que a resposta contém dois cabeçalhos personalizados que foram configurados através de políticas com propriedades.

![Portal do programador][api-management-send-results]

Se observar o [rastreio API Inspector](api-management-howto-api-inspector.md) para uma chamada de que inclui as duas políticas de exemplo anterior com propriedades, pode ver as duas `set-header` políticas com os valores de propriedade inseridas, bem como a avaliação da expressão de política para a propriedade que continha a expressão de política.

![Rastreio de API Inspector][api-management-api-inspector-trace]

Enquanto os valores de propriedade podem conter expressões de política, os valores de propriedade não podem conter outras propriedades. Se o texto que contém uma referência de propriedade é utilizado para um valor de propriedade, tais como `Property value text {{MyProperty}}`, esse referência da propriedade não será substituída e serão incluída como parte do valor de propriedade.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre como trabalhar com políticas
  * [Políticas de gestão de API](api-management-howto-policies.md)
  * [Referência de políticas](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Expressões de política](https://msdn.microsoft.com/library/azure/dn910913.aspx)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

