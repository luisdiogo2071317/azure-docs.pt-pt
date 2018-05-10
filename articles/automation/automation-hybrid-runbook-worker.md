---
title: Os Runbook Workers híbridos da automatização do Azure
description: Este artigo fornece informações sobre como instalar e utilizar o trabalho de Runbook híbrida, que é uma funcionalidade da automatização do Azure que permite a execução de runbooks em computadores no seu local datacenter ou o fornecedor de nuvem.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 36fa5f7c9cedc25f7cb446a504faccfb386bd019
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatizar recursos no seu centro de dados ou a nuvem com o Runbook Worker híbrido

Os Runbooks na automatização do Azure pode não conseguir aceder a recursos em outras nuvens ou no seu ambiente no local, uma vez que são executados na nuvem do Azure. A funcionalidade de trabalho de Runbook híbrida da automatização do Azure permite-lhe executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados.

Esta funcionalidade é ilustrada na imagem seguinte:

![Descrição geral de trabalho de Runbook híbrida](media/automation-hybrid-runbook-worker/automation.png)

## <a name="hybrid-runbook-worker-groups"></a>Grupos de trabalho de Runbook híbrida

Cada Runbook Worker híbrido é um membro de um grupo de trabalho de Runbook híbrida que especificou quando instalar o agente. Um grupo pode incluir um único agente, mas pode instalar múltiplos agentes num grupo de disponibilidade elevada.

Quando inicia um runbook num Runbook Worker híbrido, especifique o grupo que é executada no. Cada trabalho no grupo de consulta da automatização do Azure para ver se todas as tarefas estão disponíveis. Se existir uma tarefa disponível, em seguida, o primeiro worker a obter a tarefa demora-lo. Não é possível especificar uma função de trabalho específica.

## <a name="installing-a-hybrid-runbook-worker"></a>Instalar um Runbook Worker híbrido

O processo para instalar um Runbook worker híbrido é diferente consoante o sistema operativo. A tabela seguinte contém ligações para os diferentes métodos que pode utilizar para instalar um Runbook Worker híbrido. Para instalar e configurar um Runbook Worker híbrido do Windows, existem dois métodos disponíveis. O método recomendado é utilizar um runbook da automatização para automatizar completamente o processo necessário para configurar um computador Windows. O segundo método é a seguir um procedimento passo a passo para instalar e configurar a função manualmente. Para computadores Linux, tem de executar um script de Python para instalar o agente na máquina

|SO  |Tipos de implementação  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Para gerir a configuração dos seus servidores que suportam a função de trabalho de Runbook híbrida com configuração de estado pretendido (DSC), terá de adicioná-los como nós de DSC. Para obter mais informações sobre a integração-los para a gestão com o DSC, consulte [máquinas de integração para gestão pelo Azure Automation DSC](automation-dsc-onboarding.md).
>
>Se ativar o [solução de gestão de atualizações](automation-update-management.md), qualquer computador ligado à sua área de trabalho de análise de registos é automaticamente configurado como um Runbook Worker híbrido para suportar runbooks incluídos nesta solução. No entanto, não está registado em todos os grupos de trabalho híbrida já definidos na sua conta de automatização. O computador pode ser adicionado a um grupo de trabalho de Runbook híbrida na sua conta de automatização para suportar os runbooks de automatização, desde que estiver a utilizar a mesma conta para a solução e a associação ao grupo de trabalho de Runbook híbrida. Esta funcionalidade foi adicionada à versão 7.2.12024.0 da Função de Trabalho de Runbook Híbrida.

Reveja o [informações para planear a sua rede](#network-planning) antes de começar a implementar um Runbook Worker híbrido. Depois de ter implementado com êxito um runbook worker, reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.

## <a name="removing-hybrid-runbook-worker"></a>Remover o Runbook Worker híbrido

Pode remover uma ou mais os Runbook Workers híbridos de um grupo ou pode remover o grupo dependendo dos seus requisitos. Para remover um Runbook Worker híbrido a partir de um computador no local, execute os seguintes passos:

1. No portal do Azure, navegue até à sua conta de automatização.
2. Do **definições** painel, selecione **chaves** e anote os valores de campo **URL** e **chave de acesso primária**. Estas informações são necessárias para o passo seguinte.

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
> Isto não remove o Microsoft Monitoring Agent no computador, apenas a funcionalidade e a configuração da função de trabalho de Runbook híbrida.

## <a name="remove-hybrid-worker-groups"></a>Remova os grupos de trabalho híbrida

Para remover um grupo, terá primeiro de remover o Runbook Worker híbrido cada computador que seja membro do grupo utilizando o procedimento apresentado anteriormente e, em seguida, efetuar os seguintes passos para remover o grupo.

1. Abra a conta de automatização no portal do Azure.
1. Em **automatização de processos**, selecione **grupos de trabalho híbrida**. Selecione o grupo que pretende eliminar. Depois de selecionar o grupo específico, o **grupo de trabalho híbridas** é apresentada a página de propriedades.

   ![Página de grupo de trabalho de Runbook híbrida](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

1. Na página de propriedades para o grupo selecionado, clique em **eliminar**. É apresentada uma mensagem a pedir-lhe para confirmar esta ação, selecione **Sim** se tiver a certeza de que pretende prosseguir.

   ![Caixa de diálogo de confirmação de eliminação de grupo](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Este processo pode demorar alguns segundos até ser concluído e pode acompanhar o progresso em **Notificações**, no menu.

## <a name="network-planning"></a>Configurar a rede

### <a name="hybrid-worker-role"></a>Função de trabalho híbrida

Para o Runbook Worker híbrido ligar a e registar a análise de registos, tem de ter acesso para o número de porta e os URLs descritos nesta secção. Isto é, para além de [portas e URLs necessários para o Microsoft Monitoring Agent](../log-analytics/log-analytics-agent-windows.md) para ligar ao Log Analytics.

Se utilizar um servidor proxy para comunicação entre o agente e o serviço de análise de registos, certifique-se de que os recursos adequados estão acessíveis. Se utilizar uma firewall para restringir o acesso à internet, tem de configurar a firewall para permitir o acesso.

A porta e URLs seguintes são necessários para a função Runbook Worker híbrido comunicar com a automatização:

* : Apenas TCP 443 é necessária a porta De saída acesso à internet.
* Global URL: *.azure-automation.net
* URL global da E.U.A. us Virginia: *.azure automation.us
* Serviço do agente: https://\<workspaceId\>.agentsvc.azure-automation.net

Se tiver uma conta de automatização que esteja definida para uma região específica, pode restringir a comunicação para esse datacenter regional. A tabela seguinte fornece o registo DNS para cada região.

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
> O ficheiro XML do endereço IP de Datacenter do Azure lista os intervalos de endereços IP que são utilizados nos centros de dados dos Microsoft Azure. Intervalos de computação, o SQL Server e o armazenamento são incluídos no ficheiro.
>
>Um ficheiro atualizado é publicado semanalmente. O ficheiro reflete os intervalos de implementado e as alterações futuras para os intervalos de IP. Novo intervalos que aparecem no ficheiro de não são utilizados em dos centros de dados de, pelo menos, uma semana.
>
> É uma boa ideia para transferir o ficheiro XML novo todas as semanas. Em seguida, atualize o site para identificar corretamente os serviços em execução no Azure. Utilizadores do Azure ExpressRoute devem ter em consideração que este ficheiro utilizado para atualizar o anúncio Border Gateway Protocol (BGP) de espaço do Azure a primeira semana de cada mês.

### <a name="update-management"></a>Gestão de Atualizações

Além das portas que requer que o Runbook Worker híbrido e endereços padrão, os seguintes endereços são necessários especificamente para a gestão de atualização. Comunicação para estes endereços é feita através da porta 443.

* *.ods.opinsights.azure.com
* *.oms.opinsights.azure.com
* ods.systemcenteradvisor.com
* *.blob.core.windows.net/

## <a name="troubleshooting"></a>Resolução de problemas

O Runbook Worker híbrido depende do Microsoft Monitoring Agent para comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Se o registo do worker falhar, aqui estão algumas causas possíveis para o erro:

1. A função de trabalho híbrida estiver atrás de um proxy ou firewall.

   Certifique-se de que o computador tem acesso de saída para *.azure automation.net na porta 443.

2. A função de trabalho híbrida está em execução no computador tiver menos do que os requisitos mínimos de hardware.

   Computadores com o Runbook Worker híbrido devem cumprir os requisitos mínimos de hardware antes de indicar que esta funcionalidade do anfitrião. Caso contrário, consoante a utilização de recursos de outros processos em segundo plano e a contenção causado por runbooks durante a execução, o computador fica sobrecarregado e causar atrasos de tarefa de runbook ou tempos limite.

   Confirme que o computador designado para executar a funcionalidade do Runbook Worker híbrido cumpre os requisitos mínimos de hardware. Se existir, monitorize a utilização de CPU e memória para determinar a qualquer correlação entre o desempenho dos processos de trabalho de Runbook híbrida e o Windows. Se existir memória ou pressão de CPU, isto pode indicar a necessidade de atualizar ou adicione processadores adicionais ou aumente a memória para resolver o congestionamento do recurso e resolva o erro. Em alternativa, selecione um recurso de computação diferentes que pode suportar os requisitos mínimos e escala, quando a carga de trabalho exigências indicam que um aumento é necessário.

3. O serviço Microsoft Monitoring Agent não está em execução.

   Se o serviço Windows do Microsoft Monitoring Agent não está em execução, isto impede o Runbook Worker híbrido comunicar com a automatização do Azure. Certifique-se de que o agente está em execução, introduzindo o seguinte comando do PowerShell: `get-service healthservice`. Se o serviço for parado, introduza o seguinte comando do PowerShell para iniciar o serviço: `start-service healthservice`.

4. No **aplicação e o Gestor de serviços de Logs\Operations** registo de eventos, para ver eventos 4502 e EventMessage contendo **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**com a descrição seguinte: *o certificado apresentado pelo serviço \<wsid\>. oms.opinsights.azure.com não foi emitido por uma autoridade de certificação utilizada para os serviços Microsoft. Contacte o administrador de rede para ver se estiverem a executar um proxy que intercepte a comunicação TLS/SSL. O artigo KB3126513 tem informações adicionais de resolução de problemas para problemas de conectividade.*
    Isto pode dever-se a firewall, proxy ou de rede, bloquear a comunicação com o Microsoft Azure. Certifique-se de que o computador tem acesso de saída para *.azure automation.net nas portas 443.

Os registos são armazenados localmente em cada função de trabalho híbrida no C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Pode verificar se existem quaisquer avisos ou eventos de erro escritos para o **aplicações e serviços Logs\Microsoft-SMA\Operations** e **aplicação e o Gestor de serviços de Logs\Operations** registo de eventos deverá indicar uma conectividade ou outro problema que afetam a integração de função da automatização do Azure ou o problema ao efetuar operações normais.

Para obter passos adicionais sobre como resolver problemas com a gestão de atualizações, consulte [Update Management - resolução de problemas](automation-update-management.md#troubleshooting)

## <a name="next-steps"></a>Passos Seguintes

Reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.
