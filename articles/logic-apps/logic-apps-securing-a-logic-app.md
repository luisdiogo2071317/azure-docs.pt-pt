---
title: Proteger o acesso ao Azure Logic Apps | Documentos da Microsoft
description: Proteger o acesso aos acionadores, entradas e saídas, parâmetros de ação e serviços nos fluxos de trabalho para o Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 11/22/2016
ms.openlocfilehash: 0fe35b67a424caedcea2c71885d1757943ace9d1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232601"
---
# <a name="secure-access-in-azure-logic-apps"></a>Proteger o acesso no Azure Logic Apps

Seguem-se as formas que pode proteger o acesso a diferentes componentes na sua aplicação lógica:

* Proteger o acesso para acionar um fluxo de trabalho de aplicação lógica com o acionador de pedido HTTP.
* Proteger o acesso para gerir, editar ou leitura de uma aplicação lógica.
* Proteger o acesso ao conteúdo dentro das entradas e saídas de uma aplicação lógica executam.
* Proteger a parâmetros ou entradas para ações num fluxo de trabalho de aplicação lógica.
* Acesso seguro aos serviços que recebem pedidos a partir de um fluxo de trabalho de aplicação lógica.

## <a name="secure-access-to-trigger"></a>Proteger o acesso para acionar

Quando se trabalha com uma aplicação lógica que é acionado numa solicitação HTTP ([pedir](../connectors/connectors-native-reqres.md) ou [Webhook](../connectors/connectors-native-webhook.md)), pode restringir o acesso, para que apenas os clientes autorizados capazes de acionar a aplicação lógica. Todos os pedidos para uma aplicação lógica estão encriptados e protegidos através de SSL.

### <a name="shared-access-signature"></a>Assinatura de acesso partilhado

Cada ponto de extremidade do pedido para uma aplicação lógica inclui um [assinatura de acesso partilhado (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) como parte do URL. Cada URL contém um `sp`, `sv`, e `sig` parâmetro de consulta. As permissões são especificadas pela `sp`e correspondem aos métodos HTTP permitidos, `sv` é a versão utilizada para gerar, e `sig` é utilizado para autenticar o acesso a acionar. A assinatura é gerada usando o algoritmo SHA256 com uma chave secreta em todos os caminhos de URL e propriedades. A chave secreta nunca é exposta e publicada e são mantidos encriptados e armazenados como parte da aplicação lógica. A aplicação lógica autoriza apenas acionadores que contêm uma assinatura válida criada com a chave secreta.

#### <a name="regenerate-access-keys"></a>Voltar a gerar chaves de acesso

Pode voltar a gerar uma nova chave segura em qualquer altura através do portal de REST API ou do Azure. Todos os URLs atuais que foram gerados anteriormente utilizando a chave antiga são também invalidados e já não estão autorizados para acionar a aplicação lógica.

1. No portal do Azure, abra a aplicação lógica que pretende regenerar uma chave
1. Clique nas **chaves de acesso** item de menu **definições**
1. Escolha a chave para voltar a gerar e concluir o processo

Obter após a regeneração de URLs são assinados com a nova chave de acesso.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Criação de URLs de retorno de chamada com uma data de expiração

Se estiver a partilhar o URL com outras partes, pode gerar URLs com chaves específicas e datas de expiração, conforme necessário. Pode, em seguida, forma totalmente integrada distribuir chaves ou certifique-se de que o acesso para acionar uma aplicação é restrito a um determinado intervalo de tempo. Pode especificar uma data de expiração para um URL através da [aplicações lógicas REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

No corpo, incluir a propriedade `NotAfter` como uma cadeia de data do JSON, que retorna um URL de retorno de chamada que só é válido até o `NotAfter` data e hora.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>A criação de URLs com a chave secreta primária ou secundária

Ao gerar ou URLs de retorno de chamada para acionadores baseados em pedido de lista, também pode especificar qual a chave a utilizar para iniciar o URL.  Pode gerar uma URL assinada por uma chave específica através da [aplicações lógicas REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) da seguinte forma:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

No corpo, incluir a propriedade `KeyType` dado `Primary` ou `Secondary`.  Isso retorna uma URL assinada pela chave segura especificada.

### <a name="restrict-incoming-ip-addresses"></a>Restringir os endereços IP recebidos

Além de assinatura de acesso partilhado, pode ser útil restringir a chamar uma aplicação lógica apenas a partir de clientes específicos.  Por exemplo, se gerencia o ponto final através da gestão de API do Azure, pode restringir a aplicação lógica para apenas aceitar o pedido quando o pedido proveniente do endereço IP da instância de gestão de API.

Esta definição pode ser configurada nas definições de aplicação lógica:

1. No portal do Azure, abra a aplicação lógica que pretende adicionar restrições de endereço IP
1. Clique nas **definições de fluxo de trabalho** item de menu **definições**
1. Especificar a lista de intervalos de endereços IP para ser Aceito pelo acionador

Um intervalo IP válido assume o formato `192.168.1.1/32`. Se pretender que a aplicação lógica para acionar apenas como uma aplicação lógica aninhada, selecione o **apenas outras aplicações lógicas** opção. Esta opção escreve uma matriz vazia para o recurso, que significa apenas chamadas a partir do serviço em si (aplicações de lógica principal) são acionados com êxito.

> [!NOTE]
> Pode continuar a executar uma aplicação lógica com um acionador de pedido por meio da API de REST / gestão `/triggers/{triggerName}/run` , independentemente de IP. Este cenário requer a autenticação com base na API de REST do Azure e todos os eventos apareceria no Log de auditoria do Azure. Acesso de conjunto de políticas de controlo em conformidade.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Definir intervalos de IP na definição do recurso

Se estiver a utilizar um [modelo de implementação](logic-apps-create-deploy-template.md) para automatizar as implementações, as definições de intervalo IP podem ser configuradas no modelo de recurso.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Adicionar Azure Active Directory, OAuth ou outra segurança

Para adicionar protocolos de autorização mais por cima de uma aplicação lógica, [gestão de API do Azure](https://azure.microsoft.com/services/api-management/) oferece a monitorização avançada, segurança, política e documentação para qualquer ponto final com a capacidade de expor uma aplicação lógica como uma API. Gestão de API do Azure pode expor um ponto de extremidade público ou privado para a aplicação de lógica, que pode utilizar o Azure Active Directory, certificado, OAuth ou outros padrões de segurança. Quando é recebido um pedido, a API Management do Azure encaminha o pedido para a aplicação lógica (efetuar quaisquer transformações necessárias ou restrições em trânsito). Pode utilizar as definições de intervalo IP recebidas na aplicação lógica para permitir apenas a aplicação lógica para ser acionado da gestão de API.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Proteger o acesso para gerir ou editar aplicações lógicas

Pode restringir o acesso às operações de gestão numa aplicação lógica para que apenas utilizadores específicos ou grupos são capazes de executar operações no recurso. Aplicações lógicas utilizam do Azure [controlo de acesso baseado em funções (RBAC)](../role-based-access-control/role-assignments-portal.md) de recursos e pode ser personalizada com as mesmas ferramentas.  Existem algumas funções incorporadas, que pode atribuir membros da sua subscrição para também:

* **Contribuidor da aplicação lógica** -fornece acesso para ver, editar e atualizar uma aplicação lógica.  Não é possível remover o recurso ou efetuar operações de administrador.
* **Operador de aplicação lógica** – pode ver a aplicação lógica e histórico de execuções e ativar/desativar.  Não é possível editar ou atualizar a definição.

Também pode utilizar [bloqueio de recursos do Azure](../azure-resource-manager/resource-group-lock-resources.md) para impedir que alterar ou eliminar aplicações lógicas. Esta capacidade é valiosa para impedir os recursos de produção de alterações ou eliminações.

## <a name="secure-access-to-contents-of-the-run-history"></a>Proteger o acesso ao conteúdo do histórico de execuções

Pode restringir o acesso a conteúdo de entradas ou saídas de execuções anteriores para intervalos de endereços IP específicos.  

Todos os dados dentro de uma execução de fluxo de trabalho são encriptados em trânsito e em inatividade. Quando é feita uma chamada para o histórico de execução, o serviço autentica o pedido e fornece ligações para o pedido e resposta entradas e saídas. Esta ligação pode ser protegida para que os pedidos apenas para ver o conteúdo a partir de um intervalo de endereços IP designado devolver o conteúdo. Pode utilizar esta capacidade para controlo de acesso adicionais. Pode ainda especificar um endereço IP, como `0.0.0.0` para que ninguém pode aceder a entradas/saídas. Somente alguém com permissões de administrador foi possível remover esta restrição, fornecendo a possibilidade de acesso "just-in-time" o conteúdo de fluxo de trabalho.

Esta definição pode ser configurada nas definições de recursos do portal do Azure:

1. No portal do Azure, abra a aplicação lógica que pretende adicionar restrições de endereço IP
2. Clique nas **configuração do controlo de acesso** item de menu **definições**
3. Especificar a lista de intervalos de endereços IP de acesso a conteúdo

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Definir intervalos de IP na definição do recurso

Se estiver a utilizar um [modelo de implementação](logic-apps-create-deploy-template.md) para automatizar as implementações, as definições de intervalo IP podem ser configuradas no modelo de recurso.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Parâmetros seguros e de entradas num fluxo de trabalho

Pode querer parametrizar alguns aspectos de uma definição de fluxo de trabalho para a implantação em ambientes. Além disso, alguns parâmetros podem ser parâmetros seguros que não pretende que apareçam quando editar um fluxo de trabalho, como um ID de cliente e segredo do cliente para [autenticação do Azure Active Directory](../connectors/connectors-native-http.md#authentication) de uma ação de HTTP.

### <a name="using-parameters-and-secure-parameters"></a>Utilizar parâmetros e os parâmetros seguros

Para acessar o valor de um parâmetro de recurso no tempo de execução, o [linguagem de definição de fluxo de trabalho](https://aka.ms/logicappsdocs) fornece uma `@parameters()` operação. Além disso, pode [especificar parâmetros no modelo de implementação do recurso](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Mas se especificar o tipo de parâmetro como `securestring`, o parâmetro não ser devolvido com o restante da definição do recurso e não estará acessível ao visualizar o recurso após a implementação.

> [!NOTE]
> Se o parâmetro é utilizado no cabeçalhos ou no corpo de um pedido, o parâmetro pode estar visível acessando o histórico de execuções e o pedido de HTTP de saída. Certifique-se definir as políticas de acesso ao conteúdo em conformidade.
> Cabeçalhos de autorização nunca são visíveis através de entradas ou saídas. Então, se o segredo está a ser utilizado aqui, o segredo não é recuperável.

#### <a name="resource-deployment-template-with-secrets"></a>Modelo de implementação de recursos com segredos

O exemplo seguinte mostra uma implementação que faça referência a um parâmetro seguro de `secret` em tempo de execução. Num arquivo de parâmetros separados, pode especificar o valor de ambiente para o `secret`, ou utilize [Cofre de chaves do Azure Resource Manager](../azure-resource-manager/resource-manager-keyvault-parameter.md) obter segredos no momento da implantação.

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "secretDeploymentParam": {
         "type": "securestring"
      }
   },
   "variables": {},
   "resources": [ {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
         "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
         "definition": {
            "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
            "actions": {
               "Call_External_API": {
                  "type": "Http",
                  "inputs": {
                     "headers": {
                        "Authorization": "@parameters('secret')"
                     },
                     "body": "This is the request"
                  },
                  "runAfter": {}
               }
            },
            "parameters": {
               "secret": {
                  "type": "SecureString"
               }
            },
            "triggers": {
               "manual": {
                  "type": "Request",
                  "kind": "Http",
                  "inputs": {
                     "schema": {}
                  }
               }
            },
            "contentVersion": "1.0.0.0",
            "outputs": {}
         },
         "parameters": {
            "secret": {
               "value": "[parameters('secretDeploymentParam')]"
            }
         }
      }
   } ],
   "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Proteger o acesso aos serviços que recebem pedidos a partir de um fluxo de trabalho

Existem várias formas para ajudar a proteger qualquer ponto final que da aplicação lógica precisa acessar.

### <a name="using-authentication-on-outbound-requests"></a>Utilizar a autenticação nos pedidos de saída

Ao trabalhar com um HTTP, HTTP + Swagger (API aberta) ou ação do Webhook, pode adicionar autenticação à solicitação a ser enviada. Pode incluir a autenticação básica, a autenticação de certificado ou a autenticação do Azure Active Directory. Pode encontrar detalhes sobre como configurar esta autenticação [neste artigo](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Restringir o acesso a endereços IP de aplicação lógica

Todas as chamadas a partir das aplicações lógicas provenientes de um conjunto específico de endereços IP por região. Pode adicionar adicionais de filtragem para aceitar apenas pedidos desses endereços IP designados. Para obter uma lista os endereços IP, consulte [limites de aplicação lógica e configuração](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Conectividade no local

As aplicações lógicas proporcionam a integração com vários serviços para fornecer seguras e fiáveis no local comunicação.

#### <a name="on-premises-data-gateway"></a>Gateway de dados no local

Muitos conetores geridos para aplicações lógicas oferecem conectividade segura a sistemas no local, incluindo o sistema de ficheiros, SQL, SharePoint, DB2 e muito mais. O gateway reencaminha dados a partir de origens no local em canais encriptados através do Azure Service Bus. Todo o tráfego de origem como proteger o tráfego de saída do agente do gateway. Saiba mais sobre [como funciona o gateway de dados](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>API Management do Azure

[Gestão de API do Azure](https://azure.microsoft.com/services/api-management/) tem opções de conectividade no local, incluindo a integração de ExpressRoute e VPN de site a site para o proxy segura e comunicação para sistemas no local. No Estruturador da aplicação lógica, rapidamente, pode selecionar uma API exposta em gestão de API do Azure dentro de um fluxo de trabalho, fornecendo acesso rápido a sistemas no local.

## <a name="next-steps"></a>Passos Seguintes
[Criar um modelo de implementação](logic-apps-create-deploy-template.md)  
[Processamento de exceções](logic-apps-exception-handling.md)  
[Monitorizar as aplicações lógicas](logic-apps-monitor-your-logic-apps.md)  
[Diagnosticar problemas e falhas de aplicação lógica](logic-apps-diagnosing-failures.md)  
