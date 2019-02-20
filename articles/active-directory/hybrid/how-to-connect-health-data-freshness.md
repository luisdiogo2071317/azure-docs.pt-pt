---
title: O Azure AD Connect Health - dados de estado de funcionamento do serviço não estão à altura do alerta de data | Documentos da Microsoft
description: Este documento descreve a causa do alerta de "dados de estado de funcionamento do serviço não estão atualizados" e a resolução de problemas.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ad829b976d8b712ee8027c89fb618c6c07de1bc
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429025"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Dados de estado de funcionamento do serviço não são atualizada alerta

## <a name="overview"></a>Descrição geral
Os agentes nas máquinas no local que o Azure AD Connect Health monitorize periodicamente carrega dados para o serviço do Azure AD Connect Health. Se o serviço não receber dados de um agente, as informações apresentadas no portal será obsoletas. Para realçar o problema, o serviço irá gerar **dados de estado de funcionamento do serviço não estão atualizados** alerta. Isso é gerado quando o serviço não recebeu dados nas últimas duas horas.  

* O **aviso** alerta de estado é acionado se Connect Health não receber os elementos de dados parciais enviados do servidor de duas horas. Alerta de estado de aviso não aciona notificações por e-mail para o administrador de inquilinos.
* O **erro** alerta de estado é acionado se Connect Health não receber quaisquer elementos de dados enviados do servidor de duas horas. Acionadores de alerta de estado de erro notificações por email para o administrador de inquilinos.


## <a name="troubleshooting-steps"></a>Passos de resolução de problemas 

> [!IMPORTANT] 
> Este alerta segue Connect Health [política de retenção de dados](reference-connect-health-user-privacy.md#data-retention-policy)

* Certifique-se de que os serviços de agentes do Azure AD Connect Health estão em execução na máquina. Por exemplo, Connect Health para AD FS deve ter três serviços.  
  ![Verificar o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install5.png)

* Certifique-se entrar e cumprir os [secção de requisitos](how-to-connect-health-agent-install.md#requirements).
* Uso [ferramenta de teste de conectividade](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detetar problemas de conectividade.
* Se tiver um Proxy de HTTP, siga estes [passos de configuração](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

O painel de detalhes de alerta lista, os dados em falta, elemento (s) de um servidor. A tabela a seguir o ajudará a restringir ainda mais o problema. 
### <a name="connect-health-for-sync"></a>Connect Health para sincronização

| Elementos de dados | Passos de resolução de problemas |
| --- | --- | 
| PerfCounter | - [Conectividade de saída para o ponto final de serviço do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Inspeção do SSL para tráfego de saída é filtrada ou está desativada](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Portas de firewall no servidor que executa o agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| AadSyncService-SynchronizationRules, <br /> AadSyncService-Connectors, <br /> AadSyncService-GlobalConfigurations, <br /> AadSyncService-RunProfileResults, <br /> AadSyncService-ServiceConfigurations, <br /> AadSyncService-ServiceStatus | - [Conectividade de saída para o ponto final de serviço do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Portas de firewall no servidor que executa o agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>Connect Health para AD FS

Passos adicionais para validar para o AD FS e siga o fluxo de trabalho no [ajuda do AD FS](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Elementos de dados | Passos de resolução de problemas |
| --- | --- | 
| PerfCounter, TestResult | - [Conectividade de saída para o ponto final de serviço do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Inspeção do SSL para tráfego de saída é filtrada ou está desativada](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Portas de firewall no servidor que executa o agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
|  Adfs-UsageMetrics | Conectividade de saída com base em endereços IP, consulte [intervalos de IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>Connect Health para o ADDS

| Elementos de dados | Passos de resolução de problemas |
| --- | --- | 
| PerfCounter, Adds-TopologyInfo-Json, Common-TestData-Json | - [Conectividade de saída para o ponto final de serviço do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Portas de firewall no servidor que executa o agente](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |


## <a name="next-steps"></a>Passos Seguintes
* [FAQ do Azure AD Connect Health](reference-connect-health-faq.md)
