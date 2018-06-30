---
title: Registo do IIS no Log Analytics do Azure | Microsoft Docs
description: Serviços de informação Internet (IIS) armazena atividade do utilizador nos ficheiros de registo que podem ser recolhidos através da análise de registos.  Este artigo descreve como configurar a recolha de registos IIS e os detalhes dos registos que criarem na área de trabalho de análise de registos.
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
ms.comopnent: na
ms.openlocfilehash: 65320e7d3cc97a3d53fd1a00fbbeab5559c02fce
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133057"
---
# <a name="iis-logs-in-log-analytics"></a>Registo do IIS na análise de registos
Serviços de informação Internet (IIS) armazena atividade do utilizador nos ficheiros de registo que podem ser recolhidos através da análise de registos.  

![Registos do IIS](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configurar o IIS regista
Análise de registos recolhe as entradas de ficheiros de registo criados pelo IIS, por isso, tem de [configurar o IIS para o registo](https://technet.microsoft.com/library/hh831775.aspx).

Análise de registos só suporta ficheiros de registo do IIS armazenados num formato de W3C e não suporta campos personalizados ou avançados registo do IIS.  
Análise de registos não recolhe registos no formato nativo NCSA nem o IIS.

Configurar os registos do IIS na análise de registos do [menu dados nas definições de análise do registo](log-analytics-data-sources.md#configuring-data-sources).  Não há nenhuma configuração necessária à seleção **ficheiros de registo de W3C recolher formato IIS**.


## <a name="data-collection"></a>Recolha de dados
Análise de registos recolhe entradas de registo do IIS a partir de cada agente que é criada sempre que o registo está fechado e um novo. Frequência é controlada pelo **agenda de Rollover de ficheiro de registo** definição para o site do IIS que é uma vez por dia por predefinição. Por exemplo, se as definições estão **horária**, em seguida, análise de registos irá recolher o registo de cada hora.  Se a definição for **diária**, em seguida, análise de registos irá recolher o registo a cada 24 horas.


## <a name="iis-log-record-properties"></a>Propriedades de registo de registo do IIS
Registos do IIS tem um tipo de **W3CIISLog** e ter as propriedades na tabela seguinte:

| Propriedade | Descrição |
|:--- |:--- |
| Computador |Nome do computador que o evento foi recolhido a partir de. |
| cIP |Endereço IP do cliente. |
| csMethod |Método de pedido, tal como GET ou POST. |
| csReferer |Site que o utilizador seguidos de uma ligação de para o site atual. |
| csUserAgent |Tipo de browser do cliente. |
| csUserName |Nome do utilizador autenticado que acedeu ao servidor. Utilizadores anónimos são indicados por um hífen. |
| csUriStem |Destino do pedido, tais como uma página web. |
| csUriQuery |Consulta o, se aplicável, que o cliente estava a tentar executar. |
| ManagementGroupName |Nome do grupo de gestão de agentes do Operations Manager.  Para outros agentes, o que é AOI -\<ID da área de trabalho\> |
| RemoteIPCountry |País do endereço IP do cliente. |
| RemoteIPLatitude |Latitude do endereço IP do cliente. |
| RemoteIPLongitude |Longitude do endereço IP do cliente. |
| scStatus |Código de estado HTTP. |
| scSubStatus |Código de erro de subestado. |
| scWin32Status |Código de estado do Windows. |
| sIP |Endereço IP do servidor web. |
| SourceSystem |OpsMgr |
| Porta |Porta do servidor de cliente ligada a. |
| sSiteName |Nome do site do IIS. |
| TimeGenerated |Data e hora que de entrada foi registada. |
| TimeTaken |Período de tempo para processar o pedido em milissegundos. |

## <a name="log-searches-with-iis-logs"></a>Registo de pesquisas com registos IIS
A tabela seguinte fornece exemplos diferentes de consultas de registo que obter registos IIS.

| Consulta | Descrição |
|:--- |:--- |
| W3CIISLog |Todos os registos de registo do IIS. |
| W3CIISLog &#124; onde scStatus = = 500 |Todos os registos de registo IIS com o estado devolvido 500. |
| W3CIISLog &#124; resumir existente pelo cIP |Contagem dos IIS entradas de registo por endereço IP do cliente. |
| W3CIISLog &#124; onde csHost = = "www.contoso.com" &#124; resumir existente pelo csUriStem |Entradas pelo URL para o anfitrião www.contoso.com de registo de contagem de IIS. |
| W3CIISLog &#124; resumir sum(csBytes) por computador &#124; demorar 500000 |Total de bytes recebidos por cada computador do IIS. |

## <a name="next-steps"></a>Passos Seguintes
* Configurar a análise de registos para recolher outros [origens de dados](log-analytics-data-sources.md) para análise.
* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções.
* Configure alertas na análise de registos para proativamente notificá-lo de condições importantes encontradas nos registos IIS.
