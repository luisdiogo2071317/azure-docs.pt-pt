---
title: Resolver problemas de agente do Linux de análise de registos do Azure | Documentos da Microsoft
description: Descreva os sintomas, causas e resolução de problemas mais comuns com o agente Linux do Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: baa81a52d4b007cd690a2b01df642cd3775f7d6b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044141"
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Como resolver problemas com o agente do Linux para o Log Analytics

Este artigo disponibiliza ajuda a resolver problemas de erros, poderá deparar-se com o agente do Linux para o Log Analytics e sugere possíveis soluções para resolvê-los.

## <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problema: Não é possível ligar através do proxy para o Log Analytics

### <a name="probable-causes"></a>Causas prováveis
* O proxy especificado durante a integração estava incorreto
* O Log Analytics e os pontos finais de serviço de automatização do Azure não estão na lista de permissões no seu datacenter 

### <a name="resolutions"></a>Resoluções
1. Reonboard para o serviço Log Analytics com o agente do OMS para Linux ao utilizar o seguinte comando com a opção `-v` ativada. Isso permite que a saída detalhada do agente ligar através do proxy para o serviço OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Reveja a secção [atualizar as definições de proxy](log-analytics-agent-manage.md#update-proxy-settings) para verificar se configurou corretamente o agente para comunicar através de um servidor proxy.    
* Volte a verificar que os pontos de extremidade de serviço seguintes do Log Analytics estão na lista de permissões:

    |Recursos do Agente| Portas | Direção |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Porta 443| Entrada e saída |  
    |*.oms.opinsights.azure.com | Porta 443| Entrada e saída |  
    |*.blob.core.windows.net | Porta 443| Entrada e saída |  
    |*.azure-automation.net | Porta 443| Entrada e saída | 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problema: Recebe um erro 403 quando tentar carregar

### <a name="probable-causes"></a>Causas prováveis
* Data e hora é incorreta no servidor Linux 
* ID de área de trabalho e a chave de área de trabalho utilizado não estiverem corretos

### <a name="resolution"></a>Resolução

1. Verifique a hora no seu servidor do Linux com a data de comando. Se o tempo é + /-15 minutos da hora atual, em seguida, integração falha. Para correto isso atualizar a data e/ou do fuso horário do seu servidor do Linux. 
2. Certifique-se de que instalou a versão mais recente do agente do OMS para Linux.  A versão mais recente agora notifica que se desfasamento de tempo está a causar a falha de inclusão.
3. Reonboard com o ID de área de trabalho correto e a chave de área de trabalho seguindo as instruções de instalação no início deste tópico.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problema: Vê um erro 404 e 500 no ficheiro de registo logo após a integração
Este é um problema conhecido que ocorre no primeiro carregamento de dados do Linux para uma área de trabalho do Log Analytics. Isso não afeta os dados que está a ser enviada ou serviço experiência.

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problema: Não está a ver todos os dados no portal do Azure

### <a name="probable-causes"></a>Causas prováveis

- Falha de adesão ao serviço Log Analytics
- Ligação para o serviço Log Analytics é bloqueada
- Agente do OMS para Linux de dados é uma cópia de segurança

### <a name="resolutions"></a>Resoluções
1. Verifique se a integração o serviço Log Analytics foi concluída com êxito, verificando se existe o ficheiro seguinte: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard usando o `omsadmin.sh` instruções da linha de comandos
3. Se utilizar um proxy, consulte os passos de resolução de proxy fornecidos anteriormente.
4. Em alguns casos, quando o agente do OMS para Linux não consegue comunicar com o serviço, dados no agente é colocado em fila para o tamanho do buffer cheio, o que é a 50 MB. O agente do OMS para Linux deve ser reiniciado executando o seguinte comando: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Este problema ser corrigido no 1.1.0-28 de versão do agente e versões posteriores.

