---
title: Runbook Worker híbrido da automatização do Azure
description: Este artigo fornece informações sobre como instalar e utilizar o Runbook Worker híbrido, que é uma funcionalidade da automatização do Azure que pode utilizar para executar runbooks em computadores no seu local datacenter ou o fornecedor de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: be79f0111cb569509cb05b24c99f86d4ca9534b0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063930"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Automatizar recursos no seu centro de dados ou na nuvem utilizando o Runbook Worker híbrido

Os Runbooks na automatização do Azure poderá não conseguir aceder a recursos em outras nuvens ou no seu ambiente no local, porque são executadas na plataforma em nuvem do Azure. Pode utilizar a funcionalidade de trabalho de Runbook híbrida da automatização do Azure para executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados.

A imagem seguinte ilustra esta funcionalidade:

![Descrição geral das Funções de Trabalho de Runbook Híbridas (Hybrid Runbook Worker overview)](media/automation-hybrid-runbook-worker/automation.png)

Cada Runbook Worker híbrido é um membro de um grupo de trabalho de Runbook híbrida que especificou quando instalar o agente. Um grupo pode incluir um único agente, mas pode instalar múltiplos agentes num grupo de disponibilidade elevada.

Quando inicia um runbook num Runbook Worker híbrido, especifique o grupo que é executada no. Cada trabalho no grupo de consulta da automatização do Azure para ver se todas as tarefas estão disponíveis. Se uma tarefa não estiver disponível, o primeiro worker a obter a tarefa demora-lo. Não é possível especificar uma função de trabalho específica.

## <a name="install-a-hybrid-runbook-worker"></a>Instalar um Runbook Worker híbrido

O processo de instalação de um Runbook Worker híbrido depende do sistema operativo. A tabela seguinte contém ligações para os métodos que pode utilizar para a instalação. 

Para instalar e configurar um Runbook Worker híbrido do Windows, pode utilizar dois métodos. O método recomendado é utilizar um runbook da automatização para automatizar completamente o processo de configuração de um computador com o Windows. O segundo método é a seguir um procedimento passo a passo para instalar e configurar a função manualmente. Para computadores Linux, executar um script do Python para instalar o agente na máquina.

|SO  |Tipos de implementação  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Para gerir a configuração dos seus servidores que suportam a função de trabalho de Runbook híbrida com configuração de estado pretendido (DSC), terá de adicioná-los como nós de DSC. Para obter mais informações sobre a integração-los para a gestão com o DSC, consulte [máquinas de integração para gestão pelo Azure Automation DSC](automation-dsc-onboarding.md).
>
>Se ativar o [solução de gestão de atualizações](automation-update-management.md), qualquer computador que está ligado à sua área de trabalho do Log Analytics do Azure é automaticamente configurado como um Runbook Worker híbrido para suportar runbooks incluídos nesta solução. No entanto, o computador não está registado em todos os grupos de trabalho híbrida já definidos na sua conta de automatização. O computador pode ser adicionado a um grupo de trabalho de Runbook híbrida na sua conta de automatização para suportar os runbooks de automatização, desde que estiver a utilizar a mesma conta para a solução e a associação ao grupo de trabalho de Runbook híbrida. Esta funcionalidade foi adicionada à versão 7.2.12024.0 do Runbook Worker híbrido.

Reveja o [informações para planear a sua rede](#network-planning) antes de começar a implementar um Runbook Worker híbrido. Depois de implementar com êxito o trabalho, reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.

## <a name="remove-a-hybrid-runbook-worker"></a>Remover um Runbook Worker híbrido

Pode remover uma ou mais os Runbook Workers híbridos de um grupo ou pode remover o grupo dependendo dos seus requisitos. Para remover um Runbook Worker híbrido a partir de um computador no local, execute os seguintes passos:

1. No portal do Azure, aceda à sua conta de automatização.
2. Em **definições**, selecione **chaves** e anote os valores para **URL** e **chave de acesso primária**. Estas informações são necessárias para o passo seguinte.

### <a name="windows"></a>Windows

Abra uma sessão do PowerShell no modo de administrador e execute o seguinte comando. Utilize o **-Verbose** mudar para um registo detalhado do processo de remoção.

```powershell
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Para remover obsoletas máquinas do seu grupo de trabalho híbrida, utilize a opção `machineName` parâmetro.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Este código não remover o Microsoft Monitoring Agent no computador, apenas a funcionalidade e a configuração da função de trabalho de Runbook híbrida.

## <a name="remove-a-hybrid-worker-group"></a>Remover um grupo de trabalho híbrida

Para remover um grupo, terá primeiro de remover o Runbook Worker híbrido cada computador que seja um membro do grupo, utilizando o procedimento apresentado anteriormente. Em seguida, execute os seguintes passos para remover o grupo:

1. Abra a conta de automatização no portal do Azure.
1. Em **automatização de processos**, selecione **grupos de trabalho híbrida**. Selecione o grupo que pretende eliminar. É apresentada a página de propriedades desse grupo.

   ![Página Propriedades](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Na página de propriedades para o grupo selecionado, selecione **eliminar**. Uma mensagem pede-lhe para confirmar esta ação. Selecione **Sim** se tiver a certeza de que pretende continuar.

   ![Mensagem de confirmação](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Este processo pode demorar vários segundos a concluir. Pode acompanhar o progresso em **Notificações**, no menu.

## <a name="network-planning"></a>Configurar a rede

### <a name="hybrid-worker-role"></a>Função de trabalho híbrida

Para o Runbook Worker híbrido ligar a e registar a análise de registos, tem de ter acesso para o número de porta e os URLs descritos nesta secção. Este acesso é, para além de [portas e URLs necessários para o Microsoft Monitoring Agent](../log-analytics/log-analytics-agent-windows.md) para ligar ao Log Analytics.

Se utilizar um servidor proxy para comunicação entre o agente e o serviço de análise de registos, certifique-se de que os recursos adequados estão acessíveis. Se utilizar uma firewall para restringir o acesso à internet, tem de configurar a firewall para permitir o acesso.

A porta e URLs seguintes são necessários para a função Runbook Worker híbrido comunicar com a automatização:

* : Apenas TCP 443 é necessária a porta De saída acesso à internet.
* Global URL: *.azure-automation.net
* URL global da E.U.A. us Virginia: *.azure automation.us
* Serviço de agente: https://\<workspaceId\>.agentsvc.azure-automation.net

Se tiver uma conta de automatização que esteja definida para uma região específica, pode restringir a comunicação para esse datacenter regional. A tabela seguinte fornece o registo DNS para cada região:

| **Região** | **Registo de DNS** |
| --- | --- |
| EUA Centro-Oeste | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| EUA Centro-Sul |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| EUA Leste 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Canadá Central |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa Ocidental |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Europa do Norte |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Sudeste Asiático |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Índia Central |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Leste do Japão |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Sudeste da Austrália |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Reino Unido Sul | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| Gov (US) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Para obter uma lista de endereços IP da região em vez de nomes de região, transfira o [endereço IP de Datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653) ficheiro XML do Microsoft Download Center.

> [!NOTE]
> O ficheiro XML do endereço IP de Datacenter do Azure lista os intervalos de endereços IP que são utilizados nos centros de dados dos Microsoft Azure. O ficheiro inclui os intervalos de computação, o SQL Server e o armazenamento.
>
>Um ficheiro atualizado é publicado semanalmente. O ficheiro reflete os intervalos de implementado e as alterações futuras para os intervalos de IP. Novo intervalos que aparecem no ficheiro de não são utilizados em dos centros de dados de, pelo menos, uma semana.
>
> É uma boa ideia para transferir o ficheiro XML novo todas as semanas. Em seguida, atualize o site para identificar corretamente os serviços em execução no Azure. Utilizadores do Azure ExpressRoute devem ter em consideração que este ficheiro é utilizado para atualizar o anúncio de Border Gateway Protocol (BGP) de espaço do Azure na primeira semana de cada mês.

### <a name="update-management"></a>Gestão de Atualizações

Além das portas que requer que o Runbook Worker híbrido e endereços padrão, os seguintes endereços são necessários especificamente para a gestão de atualização. Comunicação para estes endereços é feita através da porta 443.

|Público do Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

## <a name="troubleshoot"></a>Resolução de problemas

Para saber como resolver os Runbook Workers híbridos, consulte [resolução de problemas os Runbook Workers híbridos](troubleshoot/hybrid-runbook-worker.md#general)

## <a name="next-steps"></a>Passos Seguintes

Para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem, consulte [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md).