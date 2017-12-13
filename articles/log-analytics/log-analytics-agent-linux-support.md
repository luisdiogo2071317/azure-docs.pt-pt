---
title: "Resolver problemas de agente do Linux de análise de registos do Azure | Microsoft Docs"
description: "Descrevem os sintomas, faz com que e resolução de problemas mais comuns com o agente Linux de análise do registo."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: magoedte
ms.openlocfilehash: 4f9eaa330ba90052080fc18def974d5dec43c1e0
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Como resolver problemas com o agente Linux para análise de registos

Este artigo fornece erros podem ocorrer com o agente Linux para análise de registos e sugere possíveis soluções para resolvê-los de resolução de problemas.

### <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problema: Não é possível ligar através do proxy para análise de registos

#### <a name="probable-causes"></a>Causas prováveis
* O proxy especificado durante a integração estava incorreto
* A análise de registos e pontos finais de serviço de automatização do Azure não estão na lista de permissões no seu centro de dados 

#### <a name="resolutions"></a>Resoluções
1. Reonboard para o serviço de análise de registos com o agente do OMS para Linux ao utilizar o seguinte comando com a opção `-v` ativada. Isto permite uma saída verbosa do agente que se ligam através do proxy para o serviço do OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Reveja a secção [configurar o agente para utilização com um servidor proxy ou Gateway do OMS](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) para verificar que configurou corretamente o agente para comunicar através de um servidor proxy.    
* Volte a verificar que os pontos finais de serviço de análise de registos seguintes estão na lista de permissões:

    |Recursos do Agente| Portas |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Porta 443|   
    |*.oms.opinsights.azure.com | Porta 443|   
    |ods.systemcenteradvisor.com | Porta 443|   
    |*.blob.Core.Windows.NET/ | Porta 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problema: Recebe um erro 403 ao tentar carregar

#### <a name="probable-causes"></a>Causas prováveis
* Data e hora está incorreta no servidor do Linux 
* ID de área de trabalho e a chave de área de trabalho utilizado não estão corretos

#### <a name="resolution"></a>Resolução

1. Verifique a hora no seu servidor Linux com a data de comando. Se a hora é + /-15 minutos da hora atual, em seguida, integração irá falhar. Para correto isto atualizar a data e/ou o fuso horário do servidor Linux. 
2. Certifique-se de que instalou a versão mais recente do agente do OMS para Linux.  A versão mais recente notifica agora, se o tempo de desfasamento está a causar a falha de integração.
3. Reonboard utilizando correto ID e a chave de área de trabalho seguir as instruções de instalação anterior deste tópico.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problema: Vir um erro 404 e 500 no ficheiro de registo imediatamente após a integração
Este é um problema conhecido que ocorre no primeiro carregamento de dados de Linux para uma área de trabalho de análise de registos. Isto não afeta os dados que está a ser enviada ou serviço experiência.

### <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problema: Não vir quaisquer dados no portal do Azure

#### <a name="probable-causes"></a>Causas prováveis

- Falha de integração para o serviço de análise de registos
- Ligação ao serviço de análise de registos está bloqueada
- Agente do OMS Linux dados de cópia de segurança

#### <a name="resolutions"></a>Resoluções
1. Verifique se a integração o serviço de análise de registos foi concluída com êxito, verificando se o ficheiro seguinte:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard utilizando o `omsadmin.sh` instruções da linha de comandos
3. Se utilizar um proxy, consulte os passos de resolução de proxy fornecidos anteriormente.
4. Em alguns casos, quando o agente do OMS para Linux não é possível comunicar com o serviço, dados no agente é colocado em fila para o tamanho de memória intermédia completa, o que é 50 MB. O agente do OMS para Linux deve ser reiniciado, executando o seguinte comando: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Este problema ser corrigido 1.1.0-28 de versão do agente e mais tarde.

### <a name="issue-omsagent-creates-excessive-number-of-user-process-on-computer-and-never-terminates-them"></a>Problema: OMSAgent cria um número excessivo de processo do utilizador no computador e nunca termina-las
Quando ativa as soluções de gestão que suportam a gerir as VMs do Linux, inicia um número de processos no agente Linux. Mas antes do processo termina, o outro processo é iniciado devido a um problema conhecido. 

#### <a name="resolutions"></a>Resoluções
Para alterar o número de processos de utilizador que podem ser geradas pelo OMSAgent, configure o agente utilizando omsadmin.sh.  O número de processos que são gerados por predefinição é 75 e antes de alterar o limite, primeiro, deve executar o seguinte comando para ver quantas OMSAgent processos atualmente em execução: `ps aux | grep -E '^omsagent' | wc -l`.  
Pode verificar que o limite atual está definido como executando o seguinte comando:`cat /etc/security/limits.conf | grep -E '^omsagent'`

Utilize os seguintes comandos para optar por configurar um limite de processo personalizado ou para definir o limite de processo no respectivo valor predefinido.

1. Para definir o limite de processo para OMSAgent: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh -n <specific number limit>`.<br>Tenha em atenção que o limite mínimo que pode ser definido para 5.  

2. Para definir o limite de processo para OMSAgent novamente para o valor predefinido:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh -N`

Certifique-se de que a nova definição foi aplicada ao executar o seguinte comando: `cat /etc/security/limits.conf | grep -E '^omsagent'`.  Se não vir a nova configuração aplicada, poderá ser porque a definir o limite de processo demasiado baixa.  