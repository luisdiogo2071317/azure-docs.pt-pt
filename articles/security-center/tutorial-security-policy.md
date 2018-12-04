---
title: Editar as políticas de segurança no Azure Policy | Documentos da Microsoft
description: Edite as políticas de segurança na política do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: rkarlin
ms.openlocfilehash: d6cc216f71efcd3b3973cd37349dd5145237f02f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839335"
---
# <a name="edit-security-policies-in-azure-policy"></a>Editar as políticas de segurança no Azure Policy
Centro de segurança ajuda-o a ver o estado das políticas de segurança e como são aplicadas em suas cargas de trabalho. Centro de segurança do Azure atribui automaticamente seus [políticas de segurança incorporadas](security-center-policy-definitions.md) em cada subscrição que está incluído. Pode configurá-las no [do Azure Policy](../azure-policy/azure-policy-introduction.md), ou utilizando a API de REST, que também permite-lhe definir políticas em grupos de gestão e em várias subscrições. Para obter mais informações, veja [Integrar políticas de segurança do Centro de Segurança no Azure Policy](security-center-azure-policy.md). Neste tutorial, vai aprender a:

> [!div class="checklist"]
> * Configurar uma política de segurança com a API REST
> * Avaliar a segurança dos seus recursos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para acompanhar as funcionalidades abrangidas neste tutorial, tem de estar no escalão de preço Standard do Centro de Segurança. Pode experimentar o Centro de segurança Standard sem encargos. Para saber mais, veja a [página de preços](https://azure.microsoft.com/pricing/details/security-center/). O início rápido [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Incluir a sua subscrição do Azure no Centro de Segurança Standard) explica-lhe como atualizar para Standard.

## <a name="configure-a-security-policy-using-the-rest-api"></a>Configurar uma política de segurança com a API REST

Como parte da integração nativa com o Azure Policy, o Centro de segurança do Azure permite-lhe tirar partido do Azure Policy API de REST para criar atribuições de política. As instruções seguintes explicam como criação de atribuições de política, bem como a personalização de atribuições existentes. 

Conceitos importantes na política do Azure: 

- R **definição de política** é uma regra 

- Uma **iniciativa** é uma coleção de definições de política (regras) 

- Uma **atribuição** é uma aplicação de uma iniciativa ou uma política para um âmbito específico (grupo de gestão, subscrição, etc.) 

Centro de segurança tem uma iniciativa incorporada que inclui todas as políticas de segurança. Para avaliar as políticas do Centro de segurança nos recursos do Azure, deve criar uma atribuição no grupo de gestão ou assinatura que deseja avaliar.  

A iniciativa incorporada tem todas as políticas do Centro de segurança ativadas por predefinição. Pode optar por desabilitar determinadas políticas da iniciativa de incorporada, por exemplo pode aplicar todas as políticas do Centro de segurança, exceto **firewall de aplicações web**, alterando o valor do parâmetro de efeito da política para  **Desativado**. 

### <a name="api-examples"></a>Exemplos de API

Nos exemplos a seguir, substitua estas variáveis:

- **{âmbito}**  introduza o nome do grupo de gestão ou subscrição se aplicar a política.
- **{poicyAssignmentName}**  introduza o [nome da atribuição de política relevante](#policy-names).
- **{name}**  introduza o nome ou o nome do administrador que aprovou a alteração de política.

Este exemplo mostra como atribuir a iniciativa de centro de segurança incorporada numa subscrição ou grupo de gestão
 
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 

Este exemplo mostra como atribuir a iniciativa de centro de segurança incorporada numa assinatura, com as seguintes políticas desativadas: 

- Atualizações do sistema ("systemUpdatesMonitoringEffect") 

- Configurações de segurança ("systemConfigurationsMonitoringEffect") 

- Proteção de ponto final ("endpointProtectionMonitoringEffect") 

 
      PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

      Request Body (JSON) 

      { 

        "properties":{ 

      "displayName":"Enable Monitoring in Azure Security Center", 

      "metadata":{ 

      "assignedBy":"{Name}" 

      }, 

      "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

      "parameters":{ 

      "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 

      "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 

      "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 

      }, 

       } 

      } 

Este exemplo mostra como remover uma atribuição:

    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 


## Referência de política de nomes <a name="policy-names"></a>

|Nome da política no Centro de segurança|Nome da política apresentado na política do Azure |Nome do parâmetro de efeito de política|
|----|----|----|
|Encriptação SQL |Monitorizar a base de dados SQL não encriptada no Centro de segurança do Azure |sqlEncryptionMonitoringEffect| 
|Auditoria do SQL |Monitorizar a base de dados SQL não auditada no Centro de segurança do Azure |sqlAuditingMonitoringEffect|
|Atualizações do sistema |Monitorizar atualizações de sistema em falta no Centro de segurança do Azure |systemUpdatesMonitoringEffect|
|Encriptação do armazenamento |Auditar encriptação de BLOBs em falta para contas de armazenamento |storageEncryptionMonitoringEffect|
|Acesso à rede JIT |Monitorizar o acesso de apenas no Time (JIT) de rede possível no Centro de segurança do Azure |jitNetworkAccessMonitoringEffect |
|Controlos de aplicações adaptáveis |Monitorizar aplicação possíveis listas de permissões no Centro de segurança do Azure |adaptiveApplicationControlsMonitoringEffect|
|Grupos de segurança de rede |Monitorizar o acesso de rede permissivo no Centro de segurança do Azure |networkSecurityGroupsMonitoringEffect| 
|Configurações de segurança |Monitorizar vulnerabilidades do SO no Centro de segurança do Azure |systemConfigurationsMonitoringEffect| 
|Endpoint protection |Monitorizar o Endpoint Protection em falta no Centro de segurança do Azure |endpointProtectionMonitoringEffect |
|Encriptação de disco |Monitorizar discos de VM não encriptados no Centro de segurança do Azure |diskEncryptionMonitoringEffect|
|Avaliação de vulnerabilidades |Monitor de vulnerabilidades de VM no Centro de segurança do Azure |vulnerabilityAssesmentMonitoringEffect|
|Firewall de aplicação Web |Monitorizar aplicações web desprotegidas no Centro de segurança do Azure |webApplicationFirewallMonitoringEffect |
|Firewall da próxima geração |Monitorizar pontos finais de redes desprotegidos no Centro de segurança do Azure| |





## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como editar as políticas de segurança na política do Azure. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Guia de operações e planeamento do Centro de Segurança do Azure](security-center-planning-and-operations-guide.md): saiba como planear e compreender as considerações de conceção do Centro de Segurança do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md): saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança do Azure): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Ganhar visibilidade em todo o inquilino para o Centro de Segurança do Azure](security-center-management-groups.md): saiba como configurar grupos de gestão para o Centro de Segurança do Azure.
* [Azure Security Center FAQ](security-center-faq.md) (FAQ do Centro de Segurança do Azure): obtenha respostas às perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.

Para saber mais sobre o Azure Policy, veja [O que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)
