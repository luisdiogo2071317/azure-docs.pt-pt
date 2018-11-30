---
title: Registos de IIS no Log Analytics do Azure | Documentos da Microsoft
description: Serviços de informação Internet (IIS) armazena a atividade do utilizador nos ficheiros de registo que podem ser recolhidos pelo Log Analytics.  Este artigo descreve como configurar a coleção de registos do IIS e os detalhes dos registos que criaram na área de trabalho do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: bwren
ms.comopnent: ''
ms.openlocfilehash: e33c30f9de56c4c5dd5f898a6a5136bbcef36c18
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336913"
---
# <a name="iis-logs-in-log-analytics"></a>Registo do IIS no Log Analytics
Serviços de informação Internet (IIS) armazena a atividade do utilizador nos ficheiros de registo que podem ser recolhidos pelo Log Analytics.  

![Registos do IIS](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Registos de configuração do IIS
O log Analytics recolhe as entradas de ficheiros de registo criados pelo IIS, assim, tem [configurar o IIS para o registo de](https://technet.microsoft.com/library/hh831775.aspx).

O log Analytics só suporta ficheiros de registo do IIS armazenados no formato W3C e não suporta campos personalizados ou avançados o registo do IIS.  
O log Analytics não recolhe registos no formato nativo NCSA ou IIS.

Configurar os registos do IIS no Log Analytics a partir do [menu de dados nas definições do Log Analytics](agent-data-sources.md#configuring-data-sources).  É necessária nenhuma configuração que selecionar **ficheiros de registo IIS de formato W3C</a&gt recolher**.


## <a name="data-collection"></a>Recolha de dados
O log Analytics recolhe entradas de registo IIS por cada agente que é criada sempre que o log esteja fechado e um novo. Esta frequência é controlada pelos **agenda de Rollover de ficheiro de registo** definição para o site do IIS que é uma vez por dia por predefinição. Por exemplo, se as definições são **por hora**, em seguida, o Log Analytics irá recolher o registo de cada hora.  Se a definição for **diária**, em seguida, o Log Analytics irá recolher o registo a cada 24 horas.


## <a name="iis-log-record-properties"></a>Propriedades de registo do registo IIS
Registros de log do IIS têm um tipo de **W3CIISLog** e ter as propriedades na tabela a seguir:

| Propriedade | Descrição |
|:--- |:--- |
| Computador |Nome do computador que o evento foi recolhido a partir de. |
| cIP |Endereço IP do cliente. |
| csMethod |Método do pedido, como GET ou POST. |
| csReferer |Site que o utilizador seguida de uma ligação de para o site atual. |
| csUserAgent |Tipo de navegador do cliente. |
| csUserName |Nome do utilizador autenticado que acedeu ao servidor. Utilizadores anónimos são indicados por um hífen. |
| csUriStem |Destino do pedido, como uma página da web. |
| csUriQuery |Consulte, se houver, o cliente estava a tentar realizar. |
| ManagementGroupName |Nome do grupo de gestão para agentes do Operations Manager.  Para outros agentes, é AOI -\<ID da área de trabalho\> |
| RemoteIPCountry |País do endereço IP do cliente. |
| RemoteIPLatitude |Latitude do endereço IP de cliente. |
| RemoteIPLongitude |Longitude do endereço IP de cliente. |
| scStatus |Código de estado HTTP. |
| scSubStatus |Código de erro de subestado. |
| scWin32Status |Código de estado do Windows. |
| sIP |Endereço IP do servidor web. |
| SourceSystem |OpsMgr |
| Desporto |Porta no cliente ligado ao servidor. |
| sSiteName |Nome do site do IIS. |
| TimeGenerated |Data e hora que a entrada foi registada. |
| timeTaken |Período de tempo para processar o pedido em milissegundos. |

## <a name="log-searches-with-iis-logs"></a>Pesquisas de registos com logs do IIS
A tabela seguinte fornece exemplos diferentes de consultas de registo que obter registros de log do IIS.

| Consulta | Descrição |
|:--- |:--- |
| W3CIISLog |Todos os registros de log do IIS. |
| W3CIISLog &#124; onde scStatus = = 500 |Todos os registos de registo IIS com um status de retorno de 500. |
| W3CIISLog &#124; resumir count () por cIP |Contagem dos IIS entradas de registo por endereço IP do cliente. |
| W3CIISLog &#124; onde csHost = = "www.contoso.com" &#124; resumir count () por csUriStem |Contagem dos IIS inicie a sessão de entradas pelo URL para o anfitrião www.contoso.com. |
| W3CIISLog &#124; resumir sum(csBytes) por computador &#124; tirar 500000 |Total de bytes recebidos por cada computador do IIS. |

## <a name="next-steps"></a>Passos Seguintes
* Configurar o Log Analytics para recolher outros [origens de dados](agent-data-sources.md) para análise.
* Saiba mais sobre [pesquisas de registos](../../log-analytics/log-analytics-queries.md) para analisar os dados recolhidos a partir de origens de dados e soluções.
* Configure alertas no Log Analytics para ser notificado proativamente das condições importantes encontradas nos logs do IIS.
