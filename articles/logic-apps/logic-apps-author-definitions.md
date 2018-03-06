---
title: "Criar definições da aplicação lógica com JSON - Azure Logic Apps | Microsoft Docs"
description: "Adicionar parâmetros, processar cadeias, criar os mapas de parâmetro e obter dados com as funções de data"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 01/31/2018
ms.author: LADocs; estfan
ms.openlocfilehash: d05f7e34cbe670db6733c199e3420c810c304a84
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="build-on-your-logic-app-definition-with-json"></a>A definição da aplicação lógica com o JSON de compilação

Para efetuar mais avançadas de tarefas com [Azure Logic Apps](../logic-apps/logic-apps-overview.md), pode utilizar a vista de código para editar a definição da aplicação lógica, que utiliza o idioma JSON simple e declarativo. Se ainda não o fez, consulte primeiro [como criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md). Além disso, consulte o [completa de referência para o idioma de definição de fluxo de trabalho](http://aka.ms/logicappsdocs).

> [!NOTE]
> Algumas funcionalidades do Azure Logic Apps, como parâmetros, estão disponíveis apenas quando trabalha na vista de código para a definição da sua aplicação lógica. Os parâmetros permitem-lhe reutilizar os valores em toda a sua aplicação lógica. Por exemplo, se pretender utilizar o mesmo endereço de e-mail em várias ações, defina esse endereço de e-mail como um parâmetro.

## <a name="view-and-edit-your-logic-app-definitions-in-json"></a>Ver e editar a lógica definição da aplicação no JSON

1. Inicie sessão no [portal do Azure](https://portal.azure.com "portal do Azure").

2. No menu à esquerda, escolha **mais serviços**. Em **Integração Empresarial**, escolha **Aplicações Lógicas**. Selecione a sua aplicação lógica.

3. Partir do menu aplicação lógica, sob **ferramentas de desenvolvimento**, escolha **vista de código de aplicação lógica**.

   A janela de vista de código abre-se e mostra a definição da aplicação lógica.

## <a name="parameters"></a>Parâmetros

Os parâmetros permitem-lhe reutilizar os valores em toda a sua aplicação lógica e estão pronto para substituir os valores que podem ser alterados frequentemente. Por exemplo, se tiver um endereço de e-mail que pretende utilizar em vários locais, deve definir esse endereço de e-mail como um parâmetro. 

Os parâmetros também são úteis quando é necessário substituir os parâmetros em ambientes diferentes, saiba mais sobre [parâmetros para a implementação](#deployment-parameters) e [API REST para a documentação do Azure Logic Apps](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Os parâmetros só estão disponíveis na vista de código.

No [primeira aplicação de lógica de exemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md), criou um fluxo de trabalho que envia mensagens de correio eletrónico quando cronologia novo aparece no feed RSS de um Web site. URL do feed é codificado, pelo que este exemplo mostra como substituir o valor de consulta com um parâmetro de modo a que pode alterar o URL do feed mais facilmente.

1. Na vista de código, localizar o `parameters : {}` de objeto e adicionar um `currentFeedUrl` objeto:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. No `When_a_feed-item_is_published` ação, localizar o `queries` secção e substitua o valor de consulta com `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

   **Before**
   ``` json
   }
      "queries": {
          "feedUrl": "https://s.ch9.ms/Feeds/RSS"
       }
   },   
   ```

   **Após**
   ``` json
   }
      "queries": {
          "feedUrl": "#@{parameters('currentFeedUrl')}"
       }
   },   
   ```

   Para associar as duas ou mais cadeias, também pode utilizar o `concat` função. 
   Por exemplo, `"@concat('#',parameters('currentFeedUrl'))"` funciona com o mesmo do exemplo anterior.

3.  Quando tiver terminado, escolha **Save** (Guardar). 

Agora pode alterar o RSS do Web site através da transmissão de um URL diferente através de feed de `currentFeedURL` objeto.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Parâmetros de implementação para os diferentes ambientes

Normalmente, os ciclos de vida de implementação tem ambientes de desenvolvimento, teste e produção. Por exemplo, poderá utilizar a mesma definição da aplicação lógica em todas as estes ambientes, mas utilizar bases de dados diferentes. Da mesma forma, pode querer utilizar a mesma definição em regiões diferentes para elevada disponibilidade, mas que cada instância da aplicação lógica para utilizar a base de dados nessa região. 

> [!NOTE] 
> Este cenário é diferente do demorar parâmetros *runtime* onde deve utilizar o `trigger()` funcionar em vez disso.

Segue-se uma definição básica:

``` json
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```
O real no `PUT` pedido para aplicações lógicas, pode fornecer o parâmetro `uri`. Cada ambiente, pode fornecer um valor diferente para o `connection` parâmetro. Porque já não existe um valor predefinido, o payload da aplicação lógica requer este parâmetro:

``` json
{
    "properties": {},
        "definition": {
          /// Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Para obter mais informações, consulte o [API REST para a documentação do Azure Logic Apps](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>Cadeias de processo com as funções

As Logic Apps tem várias funções para trabalhar com cadeias. Por exemplo, suponha que pretende passar um nome de empresa a partir de uma ordem para outro sistema. No entanto, não tiver a certeza sobre o processamento adequado para a codificação de carateres. Foi possível efetuar a codificação base64 esta cadeia de mas, para evitar escapes no URL, pode substituir vários carateres em vez disso. Além disso, só precisa de uma subcadeia para o nome da empresa porque os primeiro cinco carateres não são utilizados. 

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Estes passos descrevem como neste exemplo processa esta cadeia, trabalhar a partir do interior para exterior:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Obter o [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) para o nome da empresa, por isso, obter o número total de carateres.

2. Para obter uma cadeia mais curta, subtrair `5`.

3. Obter agora um [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Iniciar no índice `5`e vá para o resto da cadeia.

4. Converter este subcadeia para um [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) cadeia.

5. Agora [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) todos os o `+` carateres com `-` carateres.

6. Por fim, [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) todos os o `/` carateres com `_` carateres.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Mapear os itens de lista para valores de propriedade, em seguida, utilize o maps como parâmetros

Para obter resultados diferentes com base em valor de uma propriedade, o pode criar um mapa que corresponda a cada valor de propriedade para um resultado, em seguida, utilizar que o mapa como um parâmetro. 

Por exemplo, este fluxo de trabalho define algumas categorias como parâmetros e um mapa que corresponda nessas categorias com um URL específico. Em primeiro lugar, o fluxo de trabalho obtém uma lista dos artigos. Em seguida, o fluxo de trabalho utiliza o mapa para localizar o URL correspondente a categoria de cada artigo.

*   O [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) função verifica se a categoria corresponde a uma categoria de definido conhecida.

*   Após obter uma categoria correspondente, o exemplo obtém o item de mapa utilizando parênteses Retos: `parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

## <a name="get-data-with-date-functions"></a>Obter dados com as funções de data

Para obter dados a partir de uma origem de dados que não suporta nativamente *acionadores*, pode utilizar data funciona para trabalhar com as horas e as datas em vez disso. Por exemplo, esta expressão localiza quanto passos este fluxo de trabalho estão a demorar, trabalhar a partir do interior para exterior:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Do `order` ação, a extrair o `startTime`. 
2. Obter a hora atual com `utcNow()`.
3. Subtrair um segundo:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   Pode utilizar outras unidades de tempo, como `minutes` ou `hours`. 

3. Agora, pode comparar estes dois valores. 

   Se o primeiro valor é menor que o segundo valor, em seguida, mais do que um segundo foi efectuada com êxito uma vez que foi colocada a ordem.

Para formatar datas, pode utilizar ao mesmo tempo cadeia. Por exemplo, para obter o RFC1123, utilize [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Saiba mais sobre [data formatação](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```


## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Executar passos com base nos valores diferentes (comutador instruções)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar e repita os passos (ciclos)](../logic-apps/logic-apps-control-flow-loops.md)
* [Executar ou merge passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
* [Executar passos com base no estado da ação agrupada (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Saiba mais sobre o [esquema de linguagem de definição de fluxo de trabalho para o Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md)
* Saiba mais sobre [ações de fluxo de trabalho e acionadores para Logic Apps do Azure](../logic-apps/logic-apps-workflow-actions-triggers.md)