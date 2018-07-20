---
title: Configurar definições de regra de deteção inteligente do Application Insights do Azure com modelos Azure Resource Manager | Documentos da Microsoft
description: Automatizar a gestão e configuração de regras de deteção inteligente do Application Insights do Azure com modelos do Azure Resource Manager
services: application-insights
documentationcenter: ''
author: harelbr
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2018
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: 79c4746916c4da39c3aed9df978ca1c3bd9ff5d9
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39164009"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Gerir regras de deteção inteligente do Application Insights com modelos Azure Resource Manager

Regras de deteção inteligente no Application Insights podem ser geridas e configurados usando [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).
Este método pode ser utilizado durante a implantação de novos recursos do Application Insights com a automatização do Azure Resource Manager ou para modificar as definições dos recursos existentes.

## <a name="smart-detection-rule-configuration"></a>Configuração de regra de deteção inteligente

Pode configurar as seguintes definições para uma regra de deteção inteligente:
- Se a regra está ativada (a predefinição é **true**.)
- Se as mensagens de correio eletrónico devem ser enviadas para os proprietários de subscrições, contribuidores e leitores quando uma deteção for encontrado (a predefinição é **true**.)
- Qualquer um dos destinatários de e-mail adicionais que deve receber uma notificação quando é encontrada uma deteção.

Para permitir a configuração das definições de regra através do Azure Resource Manager, a configuração de regra de deteção inteligente agora está disponível como um recurso interno no recurso do Application Insights, com o nome **ProactiveDetectionConfigs**.
Para obter flexibilidade máximo, cada regra de deteção inteligente pode ser configurada com definições de notificação exclusivo.

## <a name="examples"></a>Exemplos

Seguem-se alguns exemplos que mostram como configurar as definições de regras de deteção inteligente utilizando modelos Azure Resource Manager.
Todos os exemplos de fazer referência a um recurso do Application Insights com o nome _"myApplication"_, e para a "dependência de longa duração regra de deteção inteligente", que é chamado internamente _"longdependencyduration"_.
Certifique-se de substituir o nome de recurso do Application Insights e para especificar o nome interno da regra de deteção inteligente relevantes. Consulte a tabela abaixo para obter uma lista dos nomes correspondentes interno do Azure Resource Manager para cada regra de deteção inteligente.

### <a name="disable-a-smart-detection-rule"></a>Desativar uma regra de deteção inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>Desativar o envio de notificações de e-mail para uma regra de deteção inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>Adicionar destinatários de e-mail adicionais para uma regra de deteção inteligente

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```

## <a name="smart-detection-rule-names"></a>Nomes de regras de deteção inteligente

Abaixo é uma tabela de nomes da regra de deteção inteligente, tal como aparecem no portal, juntamente com seus nomes internos, que deve ser utilizado no modelo do Azure Resource Manager.

> [!NOTE]
> Regras de deteção inteligente marcadas como pré-visualização não oferecem suporte a notificações por e-mail. Por conseguinte, apenas pode definir a propriedade ativada para estas regras. 

| Nome da regra de portal do Azure | Nome interno
|:---|:---|
| Tempo de carregamento da página lento | slowpageloadtime |
| Tempo de resposta do servidor lenta | slowserverresponsetime |
| Duração longa da dependência | longdependencyduration |
| Degradação no tempo de resposta do servidor | degradationinserverresponsetime |
| Degradação na duração da dependência | degradationindependencyduration |
| Degradação na proporção de gravidade de rastreio (pré-visualização) | extension_traceseveritydetector |
| Aumento anormal no volume de exceção (pré-visualização) | extension_exceptionchangeextension |
| Potencial fuga de memória detetado (pré-visualização) | extension_memoryleakextension |
| Potencial problema de segurança detetado (pré-visualização) | extension_securityextensionspackage |
| Detetado um problema de utilização de recursos (pré-visualização) | extension_resourceutilizationextensionspackage |

## <a name="next-steps"></a>Próximos Passos

Saiba mais sobre como detetar automaticamente:

- [Anomalias de falha](app-insights-proactive-failure-diagnostics.md)
- [Vazamentos de memória](app-insights-proactive-potential-memory-leak.md)
- [Anomalias de desempenho](app-insights-proactive-performance-diagnostics.md)