---
title: Esquema de atualizações de pré-visualização de 1-Agosto de 2015 - Azure Logic Apps | Microsoft Docs
description: Criar definições de JSON para o Azure Logic Apps com o esquema versão 2015-08-01-pré-visualização
author: stepsic-microsoft-com
manager: SyntaxC4
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: stepsic; LADocs
ms.openlocfilehash: bdadc2e33082421500f21d5926ac1e660f4164d4
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Atualizações do esquema para o Azure Logic Apps - preview 1 de Agosto de 2015

Este esquema e a API versão para o Azure Logic Apps inclui melhoramentos essenciais que tornam as logic apps mais fiável e mais fáceis de utilizar:

* O **APIApp** tipo de ação tem agora o nome [ **APIConnection**](#api-connections).
* O **repetir** ação tem agora o nome [ **Foreach**](#foreach).
* O [ **serviço de escuta HTTP** aplicação API](#http-listener) já não é necessário.
* Chamar fluxos de trabalho subordinado utiliza um [novo esquema](#child-workflows).

<a name="api-connections"></a>

## <a name="move-to-api-connections"></a>Mover para ligações de API

A alteração maior é que já não tem de implementar as API Apps na sua subscrição do Azure para que possa utilizar APIs. Seguem-se as formas como pode utilizar as APIs:

* APIs geridas
* Suas APIs Web personalizado

Cada forma é processada de forma ligeiramente porque a respetiva gestão e modelos de alojamento são diferentes. Uma vantagem deste modelo é que estiver já não está restringida a recursos que são implementados no seu grupo de recursos do Azure. 

### <a name="managed-apis"></a>APIs geridas

Microsoft gere algumas APIs em seu nome, como o Office 365, Salesforce, Twitter e FTP. Pode utilizar algumas APIs geridas como-, como o Bing traduzir, enquanto outras precisam de configuração, também é denominado um *ligação*.

Por exemplo, quando utiliza o Office 365, tem de criar uma ligação que inclui o token de início de sessão do Office 365. O token de forma segura é armazenado e atualizado para que a sua aplicação lógica sempre pode chamar a API do Office 365. Se pretender ligar ao seu servidor SQL ou FTP, tem de criar uma ligação com a cadeia de ligação. 

Nesta definição, estas ações são denominadas `APIConnection`. Eis um exemplo de uma ligação que chama o Office 365 para enviar uma mensagem de e-mail:

``` json
{
   "actions": {
      "Send_an_email": {
         "type": "ApiConnection",
         "inputs": {
            "host": {
               "api": {
                  "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
               },
               "connection": {
                  "name": "@parameters('$connections')['shared_office365']['connectionId']"
               }
            },
            "method": "POST",
            "body": {
               "Subject": "Reminder",
               "Body": "Don't forget!",
               "To": "me@contoso.com"
            },
            "path": "/Mail"
         }
      }
   }
}
```

O `host` objeto faz parte das entradas que é exclusiva para ligações de API e contém estas partes: `api` e `connection`. O `api` objeto Especifica o tempo de execução do URL para onde que geridos API está alojado. Pode ver todos os disponíveis APIs geridas chamando `GET https://management.azure.com/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Quando utilizar uma API, essa API poderá ou poderá não ter definido qualquer *parâmetros de ligação*. Por isso, não se a API não definir estes parâmetros, é necessária nenhuma ligação. Se a API definir estes parâmetros, terá de criar uma ligação com um nome especificado.  
Referenciar esse nome nos, em seguida, o `connection` dentro de objeto a `host` objeto. Para criar uma ligação num grupo de recursos, chame este método:

```
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/<name>?api-version=2015-08-01-preview
```

Com o corpo do seguinte:

``` json
{
   "properties": {
      "api": {
         "id": "/subscriptions/<Azure-subscription-ID>/providers/Microsoft.Web/managedApis/azureblob"
      },
      "parameterValues": {
         "accountName": "<Azure-storage-account-name-with-different-parameters-for-each-API>"
      }
   },
   "location": "<logic-app-location>"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Implementar APIs geridas por um modelo Azure Resource Manager

Pode criar uma aplicação completa num modelo Azure Resource Manager, desde o início de sessão interativo não é necessário.
Se o início de sessão é necessário, pode configurar tudo com o modelo Azure Resource Manager, mas ainda tem de visitar o portal do Azure para autorizar as ligações. 

``` json
"resources": [ {
   "apiVersion": "2015-08-01-preview",
   "name": "azureblob",
   "type": "Microsoft.Web/connections",
   "location": "[resourceGroup().location]",
   "properties": {
      "api": {
         "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
      },
      "parameterValues": {
         "accountName": "[parameters('storageAccountName')]",
         "accessKey": "[parameters('storageAccountKey')]"
      }
    },
},
{
   "type": "Microsoft.Logic/workflows",
   "apiVersion": "2015-08-01-preview",
   "name": "[parameters('logicAppName')]",
   "location": "[resourceGroup().location]",
   "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"],
   "properties": {
      "sku": {
         "name": "[parameters('sku')]",
         "plan": {
            "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('svcPlanName'))]"
         }
      },
      "parameters": {
         "$connections": {
             "value": {
                  "azureblob": {
                     "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                     "connectionName": "azureblob",
                     "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                  }
             }
         }
      },
      "definition": {
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "type": "Object",
            "$connections": {
               "defaultValue": {},
 
            }
         },
         "triggers": {
            "Recurrence": {
               "type": "Recurrence",
               "recurrence": {
                  "frequency": "Day",
                  "interval": 1
               }
            }
         },
         "actions": {
            "Create_file": {
               "type": "ApiConnection",
               "inputs": {
                  "host": {
                     "api": {
                        "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                     },
                     "connection": {
                       "name": "@parameters('$connections')['azureblob']['connectionId']"
                     }
                  },
                  "method": "POST",
                  "queries": {
                     "folderPath": "[concat('/', parameters('containerName'))]",
                     "name": "helloworld.txt"
                  },
                  "body": "@decodeDataUri('data:, Hello+world!')",
                  "path": "/datasets/default/files"
               },
               "conditions": []
            }
         },
         "outputs": {}
      }
   }
} ]
```

Pode ver neste exemplo, as ligações são apenas recursos que residem no seu grupo de recursos. Fizerem referência as APIs geridas disponíveis na sua subscrição.

### <a name="your-custom-web-apis"></a>Suas APIs Web personalizado

Se utilizar os suas próprias APIs, aqueles não gerida pela Microsoft, utilize o incorporado **HTTP** ação chamá-los. Para uma experiência ideal, deve expor um ponto final de Swagger para a API. Este ponto final permite o Designer de aplicação lógica para compor as entradas e saídas para a API. Sem Swagger, o estruturador pode mostrar apenas as entradas e saídas como opaco objetos JSON.

Eis um exemplo que mostra o novo `metadata.apiDefinitionUrl` propriedade:

``` json
"actions": {
   "mycustomAPI": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
      },
      "inputs": {
         "uri": "https://mysite.azurewebsites.net/api/getsomedata",
         "method": "GET"
      }
   }
}
```

Se alojar a API Web no App Service do Azure, a API Web é automaticamente apresentado na lista de ações disponíveis no designer. Caso contrário, terá de colar o URL diretamente. O ponto final de Swagger tem de ser não autenticado para ser utilizado no Designer de aplicação lógica, embora possa proteger API com qualquer métodos que suporta Swagger.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Chamada implementado as API apps 2015-08-01-pré-visualização

Se tiver implementado anteriormente uma aplicação API, pode chamar essa aplicação com o **HTTP** ação.
Por exemplo, se utilizar o Dropbox para ficheiros de lista, o **pré-visualização 2014-12-01** definição da versão de esquema pode ter algo semelhante ao seguinte:

``` json
"definition": {
   "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
         "defaultValue": "eyJ0eX...wCn90",
         "type": "String",
         "metadata": {
            "token": {
               "name": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
            }
         }
      }
    },
    "actions": {
       "dropboxconnector": {
          "type": "ApiApp",
          "inputs": {
             "apiVersion": "2015-01-14",
             "host": {
                "id": "/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                "gateway": "https://avdemo.azurewebsites.net"
             },
             "operation": "ListFiles",
             "parameters": {
                "FolderPath": "/myfolder"
             },
             "authentication": {
                "type": "Raw",
                "scheme": "Zumo",
                "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
             }
          }
       }
    }
}
```

Agora, agora pode construir a ação de HTTP equivalente semelhante ao seguinte exemplo, embora mantendo a secção de parâmetros para a definição da aplicação lógica inalterado:

``` json
"actions": {
   "dropboxconnector": {
      "type": "Http",
      "metadata": {
         "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
      },
      "inputs": {
         "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
         "method": "POST",
         "body": {
            "FolderPath": "/myfolder"
         },
         "authentication": {
            "type": "Raw",
            "scheme": "Zumo",
            "parameter": "@parameters('/subscriptions/<Azure-subscription-ID>/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
         }
      }
   }
}
```

Walking através destas propriedades de um de cada:

| Propriedade de ação | Descrição |
| --- | --- |
| `type` | `Http` em vez de `APIapp` |
| `metadata.apiDefinitionUrl` | Para utilizar esta ação no Designer de aplicação lógica, incluem o ponto final de metadados, que é construído a partir de: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Construídas a partir de: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Sempre `POST` |
| `inputs.body` | Idênticos para os parâmetros de aplicação API |
| `inputs.authentication` | Idênticos para a autenticação da aplicação de API |

Esta abordagem deverão funcionar para todas as ações de aplicação API. No entanto, lembre-se de que estas aplicações API anterior já não são suportadas. Por isso, deve mover para as duas outras opções anteriores, uma API gerida ou a alojar a API Web personalizados.

<a name="foreach"></a>

## <a name="renamed-repeat-to-foreach"></a>Mudar o nome 'Repetir' para 'foreach'

Para a versão do esquema anteriores, recebemos comentários do cliente que o **repetir** nome da ação foi confuso e não foi corretamente capturar que **repetir** foi realmente um para cada ciclo. Por isso, vamos mudado `repeat` para `foreach`. Anteriormente seria escrever esta ação com este exemplo:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "repeat": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{repeatItem()}"
      }
   }
}
```

Agora teria de escrever em vez disso, esta versão:

``` json
"actions": {
   "pingBing": {
      "type": "Http",
      "foreach": "@range(0,2)",
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com/search?q=@{item()}"
      }
   }
}
```

Além disso, o `repeatItem()` função, o que referenciado o item que está a processar durante a atual iteração do ciclo, agora é alterada `item()`. 

### <a name="reference-outputs-from-foreach"></a>Saídas de referência de 'foreach'

Para a simplificação, as saídas da `foreach` ações já não são moldadas num objeto com o nome `repeatItems`. Além disso, com estas alterações, o `repeatItem()`, `repeatBody()`, e `repeatOutputs()` funções são removidas.

Por isso, utilizando anterior `repeat` exemplo, pode obter estas saídas:

``` json
"repeatItems": [ {
   "name": "pingBing",
   "inputs": {
      "uri": "https://www.bing.com/search?q=0",
      "method": "GET"
   },
   "outputs": {
      "headers": { },
      "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
   },
   "status": "Succeeded"
} ]
```

Agora pode obter estas saídas em vez disso:

``` json
[ {
   "name": "pingBing",
      "inputs": {
         "uri": "https://www.bing.com/search?q=0",
         "method": "GET"
      },
      "outputs": {
         "headers": { },
         "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
      },
      "status": "Succeeded"
} ]
```

Anteriormente, para obter o `body` da ação ao referenciar estes saídas:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "repeat": "@outputs('pingBing').repeatItems",
      "inputs": {
         "method": "POST",
         "uri": "http://www.example.com",
         "body": "@repeatItem().outputs.body"
      }
   }
}
```

Agora pode utilizar esta versão em vez disso:

``` json
"actions": {
   "secondAction": {
      "type": "Http",
      "foreach": "@outputs('pingBing')",
      "inputs": {
         "method": "POST",
         "uri": "http://www.example.com",
         "body": "@item().outputs.body"
      }
   }
}
```

<a name="http-listener"></a>

## <a name="native-http-listener"></a>Serviço de escuta HTTP nativo

As capacidades do serviço de escuta de HTTP estão agora incorporadas. Por isso, já não precisar de implementar uma aplicação de API do serviço de escuta de HTTP. Consulte [todos os detalhes sobre como efetuar o ponto final de aplicação de lógica possível chamar EndRead aqui](../logic-apps/logic-apps-http-endpoint.md). 

Com estas alterações, Removemos o `@accessKeys()` função, o que são substituídos com o `@listCallbackURL()` função para obter o ponto final quando for necessário. Além disso, tem de definir pelo menos um acionador agora na sua aplicação lógica. Se pretender `/run` o fluxo de trabalho, tem de ter um estes acionadores: `manual`, `apiConnectionWebhook`, ou `httpWebhook`.

<a name="child-workflows"></a>

## <a name="call-child-workflows"></a>Chamar fluxos de trabalho de subordinados

Anteriormente, chamar fluxos de trabalho subordinado necessário ir para o fluxo de trabalho, obter o token de acesso e colando o token a definição da aplicação lógica onde pretende chamar esse fluxo de trabalho subordinados. Com o novo esquema, o motor de Logic Apps gera automaticamente uma SAS no tempo de execução do fluxo de trabalho subordinados pelo que não terá de colar qualquer segredos para a definição. Segue-se um exemplo:

``` json
"myNestedWorkflow": {
   "type": "Workflow",
   "inputs": {
      "host": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/myWorkflow001",
         "triggerName": "myEndpointTrigger"
      },
      "queries": {
         "extrafield": "specialValue"
      },
      "headers": {
         "x-ms-date": "@utcnow()",
         "Content-type": "application/json"
      },
      "body": {
         "contentFieldOne": "value100",
         "anotherField": 10.001
      }
   },
   "conditions": []
}
```

Uma segunda melhoria é que está a dar os fluxos de trabalho subordinado acesso total para o pedido de entrada. Isto significa que pode passar parâmetros a *consultas* secção e no *cabeçalhos* objeto e que pode definir completamente o corpo de todo.

Por fim, são necessárias alterações ao fluxo de trabalho subordinados. Enquanto foi anteriormente chamar um fluxo de trabalho subordinados diretamente, agora tem de definir um ponto final de Acionador no fluxo de trabalho do elemento principal chamar. Geralmente, deverá ser adicionado um acionador que tenha `manual` escreva e, em seguida, utilizar essa acionador na definição do principal. Tenha em atenção o `host` propriedade especificamente tem um `triggerName` porque tem de especificar sempre cujo accionador que está a invocar.

## <a name="other-changes"></a>Outras alterações

### <a name="new-queries-property"></a>Nova propriedade de 'consultas'

Todos os tipos de ação suportam agora uma nova entrada chamada `queries`. Esta entrada pode ser um objeto estruturado, em vez de ter de Monte manualmente a cadeia.

### <a name="renamed-parse-function-to-json"></a>Função 'Parse()' cujo nome foi alterado para 'json()'

Que estamos a adicionar mais conteúdos tipos em breve, pelo que iremos mudado o `parse()` funcionar para `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Disponível em breve: APIs de integração do Enterprise

Temos versões geridas ainda das APIs de integração empresarial, como o AS2. Entretanto, pode utilizar os APIs de BizTalk implementada existente através da ação de HTTP. Para obter detalhes, consulte "Utilizar suas API apps já implementados" no [Roteiro da integração](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
