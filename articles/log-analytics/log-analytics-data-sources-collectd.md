---
title: Recolher dados recolhidos no Log Analytics | Documentos da Microsoft
description: Recolhidos é um daemon de Linux do código-fonte aberto que periodicamente recolhe dados de aplicativos e suas informações de nível de sistema.  Este artigo fornece informações sobre a recolha de dados recolhidos no Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: a1f28103f8faabae166f09185db3f3e1fee7a5ab
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404601"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Recolher dados de recolhidos nos agentes do Linux no Log Analytics
[Recolhidos](https://collectd.org/) é um daemon de Linux do código-fonte aberto que periodicamente recolhe métricas de desempenho de aplicativos e suas informações de nível de sistema. Aplicativos de exemplo incluem a Máquina Virtual de Java (JVM), o servidor MySQL e o Nginx. Este artigo fornece informações sobre a recolha de dados de desempenho recolhidos no Log Analytics.

Uma lista completa de plug-ins disponíveis pode ser encontrada em [tabela de plug-ins](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Descrição geral de recolhidos](media/log-analytics-data-sources-collectd/overview.png)

A seguinte configuração recolhidos está incluída no agente do Log Analytics para Linux para encaminhar os dados recolhidos para o agente do Log Analytics para Linux.

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Além disso, se utilizar um versões do recolhidos antes 5.5, utilize a seguinte configuração em vez disso.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

A configuração de recolhidos utiliza a predefinição`write_http` Plug-in para enviar dados de métrica de desempenho através da porta 26000 para agente do Log Analytics para Linux. 

> [!NOTE]
> Esta porta pode ser configurada para uma porta definida pelo personalizada, se necessário.

O agente do Log Analytics para Linux também escuta na porta 26000 para métricas recolhidos e, em seguida, converte-os para as métricas de esquema do Log Analytics. Segue-se o agente do Log Analytics para a configuração do Linux `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Versões suportadas
- Atualmente, o log Analytics suporta recolhidos versão 4.8 e acima.
- Agente do log Analytics para Linux v1.1.0-217 ou superior é necessário para a recolha de métrica recolhidos.


## <a name="configuration"></a>Configuração
Seguem-se passos básicos para configurar a recolha de dados recolhidos no Log Analytics.

1. Configure recolhidos para enviar dados para o agente do Log Analytics para Linux utilizar o plug-in de write_http.  
2. Configure o agente do Log Analytics para Linux escutar os dados recolhidos na porta adequada.
3. Reinicie o agente recolhidos e o Log Analytics para Linux.

### <a name="configure-collectd-to-forward-data"></a>Configurar recolhidos para reencaminhar dados 

1. Para encaminhar os dados recolhidos para o agente do Log Analytics para Linux, `oms.conf` precisa ser adicionado ao diretório de configuração do recolhidos. O destino deste ficheiro depende a distribuição Linux da sua máquina.

    Se o seu diretório de configuração recolhidos está localizado na /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Se o seu diretório de configuração recolhidos está localizado na /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Para versões de recolhidos antes 5.5 terá de modificar as etiquetas no `oms.conf` conforme mostrado acima.
    >

2. Copie collectd.conf para diretório de configuração da área de trabalho pretendida omsagent.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Reinicie o agente recolhidos e o Log Analytics para Linux com os seguintes comandos.

    sudo recolhidos reinício sudo /opt/microsoft/omsagent/bin/service_control reiniciar o serviço do

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a>Métricas de recolhidos para a conversão do esquema do Log Analytics
Para manter um modelo familiar entre as métricas de infraestrutura recolhidas pelo agente de Log Analytics para Linux e novas métricas recolhidas pelo recolhidos é utilizado o seguinte mapeamento do esquema:

| Campo de métrica recolhidos | Campo do log Analytics |
|:--|:--|
| anfitrião | Computador |
| Plug-in | Nenhuma |
| plugin_instance | Nome da Instância<br>Se **plugin_instance** é *nulo* , em seguida, InstanceName = "*total*" |
| tipo | ObjectName |
| type_instance | CounterName<br>Se **type_instance** é *nulo* , em seguida, CounterName =**em branco** |
| dsnames [] | CounterName |
| dstypes | Nenhuma |
| valores [] | CounterValue |

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registos](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções. 
* Uso [campos personalizados](log-analytics-custom-fields.md) para analisar dados de registos de syslog em campos individuais.

