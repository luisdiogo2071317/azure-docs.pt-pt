---
title: Integrar a solução de monitorização externa com pilha do Azure | Microsoft Docs
description: Saiba como integrar a pilha do Azure com uma solução de monitorização externa no seu centro de dados.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 05/10/2018
ms.author: jeffgilb
ms.reviewer: thoroet
ms.openlocfilehash: d7c8520602132722fd0c7138de4a276b9ac2208a
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807344"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Integrar a solução de monitorização externa com pilha do Azure

Para externo monitorização da infraestrutura de pilha do Azure, terá de monitorizar o software de pilha do Azure, os computadores físicos e os comutadores de rede física. Cada uma destas áreas oferece um método para obter informações de estado de funcionamento e o alerta:

- Software de pilha do Azure oferece uma API baseada em REST para obter o estado de funcionamento e alertas. A utilização de tecnologias definidas por software, tais como espaços de armazenamento direto, o estado de funcionamento de armazenamento e alertas fazem parte do software de monitorização.
- Computadores físicos podem disponibilizar estado de funcionamento e alertas informações através de controladores de gestão de placas base (BMCs).
- Dispositivos de rede físicos podem disponibilizar informações de alerta e de estado de funcionamento através do protocolo SNMP.

Cada solução de pilha do Azure é fornecido com um anfitrião do ciclo de vida de hardware. Este anfitrião é executado o software de monitorização do fornecedor de hardware de fabricante de equipamento Original (OEM) para os servidores físicos e os dispositivos de rede. Se assim o desejar, pode ignorar estas soluções de monitorização e diretamente integrar com soluções de monitorização existentes no seu centro de dados.

> [!IMPORTANT]
> A solução de monitorização externa que utilizar tem de ser sem agente. Não é possível instalar agentes de terceiros dentro de componentes de pilha do Azure.

O diagrama seguinte mostra o fluxo de tráfego entre um sistema de pilha do Azure integrado, o anfitrião de ciclo de vida de hardware, uma solução de monitorização externa e um sistema de coleção de emissão de permissões/dados externos.

![Diagrama que mostra o tráfego entre a pilha do Azure, monitorização e relatórios de solução.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

Este artigo explica como integrar a pilha do Azure com soluções de monitorização externas, como o System Center Operations Manager e da Nagios. Também inclui como trabalhar com alertas através de programação utilizando o PowerShell ou através de chamadas da REST API.

## <a name="integrate-with-operations-manager"></a>Integrar com o Operations Manager

Pode utilizar o Gestor de operações para monitorização externo da pilha do Azure. O pacote de gestão do System Center para a pilha do Microsoft Azure permite-lhe monitorizar múltiplas implementações de pilha do Azure com uma única instância do Operations Manager. O pacote de gestão utiliza o fornecedor de recursos de estado de funcionamento e o fornecedor de recursos de atualização REST APIs para comunicar com a pilha do Azure. Se pretender ignorar OEM monitorização software que está a ser executado no anfitrião de ciclo de vida de hardware, pode instalar pacotes de gestão do fornecedor para monitorizar os servidores físicos. Também pode utilizar a deteção de dispositivos de rede do Operations Manager para monitorizar os comutadores de rede.

O pacote de gestão para a pilha do Azure fornece as seguintes capacidades:

- Pode gerir várias implementações de pilha do Azure
- Não há suporte para o Azure Active Directory (Azure AD) e serviços de Federação do Active Directory (AD FS)
- Pode obter e fechar alertas
- Há um Estado de funcionamento e um dashboard de capacidade
- Inclui a deteção de modo de manutenção automática para quando o patch e atualização (P & U) está em curso
- Inclui tarefas de atualização de imposição para implementação e região
- Pode adicionar informações personalizadas para uma região
- Suporta notificações e relatórios

Pode transferir o pacote de gestão do System Center para a pilha do Microsoft Azure e associada [Guia do utilizador](https://www.microsoft.com/en-us/download/details.aspx?id=55184), ou diretamente a partir do Operations Manager.

Para uma solução de emissão de permissões, pode integrar o Operations Manager com o System Center Service Manager. O conector de produto integrada permite a comunicação bidirecional, que permite-lhe fechar um alerta na pilha do Azure e o Operations Manager depois de resolver um pedido de serviço no Service Manager.

O diagrama seguinte mostra a integração da pilha do Azure com uma implementação existente do System Center. Pode automatizar o Service Manager adicional com o System Center Orchestrator ou o Service Management Automation (SMA) para executar operações na pilha do Azure.

![Diagrama que mostra a integração com OM, o Service Manager e o SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integrar com da Nagios

Um Nagios Plug-in de monitorização foi desenvolvido em conjunto com soluções de Cloudbase parceiros, que está disponível com a licença de software gratuito permissiva – MIT (Massachusetts Institute of Technology).

O plug-in é escrito no Python e tira partido do fornecedor de recursos de estado de funcionamento REST API. Oferece funcionalidades básicas para obter e fechar alertas na pilha do Azure. Como o pacote de gestão do System Center, permite-lhe para adicionar múltiplas implementações de pilha do Azure e enviar notificações.

O plug-in funciona com Enterprise da Nagios e da Nagios Core. Poderá transferi-lo [aqui](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). O site de transferência inclui também detalhes da instalação e configuração.

### <a name="plugin-parameters"></a>Parâmetros de plug-in

Configure o ficheiro de plug-in "Azurestack_plugin.py" com os seguintes parâmetros:

| Parâmetro | Descrição | Exemplo |
|---------|---------|---------|
| *arm_endpoint* | Ponto final do Gestor de recursos (administrador) do Azure |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Ponto final do Gestor de recursos (administrador) do Azure  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | ID de subscrição de Admin | Obter através do portal de administrador ou o PowerShell |
| *User_name* | O nome de utilizador do operador subscrição | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | Palavra-passe de subscrição de operador | mypassword |
| *Client_id* | Cliente | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *Região* |  Nome de região de pilha do Azure | local |
|  |  |

* O GUID do PowerShell que é fornecida é universal. Pode utilizá-la para cada implementação.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>Utilizar o PowerShell para monitorizar estado de funcionamento e alertas

Se não estiver a utilizar o Operations Manager, Nagios ou uma solução baseada em da Nagios, pode utilizar o PowerShell para ativar uma vasta gama de monitorização de soluções para integrar com a pilha do Azure.

1. Para utilizar o PowerShell, certifique-se de que tem [PowerShell instalada e configurada](azure-stack-powershell-configure-quickstart.md) para um ambiente de operador de pilha do Azure. Instale o PowerShell num computador local que pode alcançar o ponto de final do Gestor de recursos (administrador) (https://adminmanagement. [ Região]. [External_FQDN]).

2. Execute os seguintes comandos para estabelecer ligação ao ambiente de pilha do Azure como um operador de pilha do Azure:

   ```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Utilizar comandos, como os exemplos seguintes para trabalhar com alertas:
   ```PowerShell
    #Retrieve all alerts
    Get-AzsAlert

    #Filter for active alerts
    $Active=Get-AzsAlert | Where {$_.State -eq "active"}
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    Get-AzsRPHealth

    #Retrieve infrastructure role instance health
    $FRPID=Get-AzsRPHealth|Where-Object {$_.DisplayName -eq "Capacity"}
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId

    ```

## <a name="learn-more"></a>Saiba mais

Para obter informações sobre a monitorização de estado de funcionamento incorporado, consulte [monitorizar o estado de funcionamento e alertas na pilha de Azure](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Passos Seguintes

[Integração de segurança](azure-stack-integrate-security.md)
