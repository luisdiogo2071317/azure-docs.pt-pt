---
title: Recomendações de desempenho do Assistente do Azure | Documentos da Microsoft
description: Utilize o Assistente para otimizar o desempenho das suas implementações do Azure.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3331c795cbb1c45820d4c86d287ef57b54f0ae6b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247648"
---
# <a name="advisor-performance-recommendations"></a>Recomendações de desempenho do Assistente

Recomendações de desempenho do Assistente do Azure ajudam a melhorar a velocidade e a capacidade de resposta das suas aplicações críticas para a empresa. Pode obter recomendações de desempenho do Advisor o **desempenho** separador do dashboard do Advisor.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Reduzir o tempo DNS ao vivo no seu perfil do Gestor de tráfego para a ativação pós-falha para pontos finais de bom estado de funcionamento mais rapidamente

[Tempo para as definições de Live (TTL)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) no perfil do Traffic Manager permitem-lhe especificar como rapidamente mudar pontos de extremidade, se um determinado ponto de final deixa de responder às consultas. Reduzir os valores TTL significa que os clientes serão encaminhados para pontos finais de funcionamento mais rápidos.

O Azure Advisor identifica perfis do Gestor de tráfego com um valor de TTL mais configurado e recomenda configurar o valor de TTL para 20 segundos ou 60 segundos, dependendo se o perfil está configurado para [rápida ativação pós-falha](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>Melhore o desempenho da base de dados com o Assistente da BD SQL

O assistente fornece-lhe uma vista consolidada consistente de recomendações para todos os seus recursos do Azure. Ele se integra com o Assistente de base de dados SQL para lhe oferecer recomendações para melhorar o desempenho da base de dados do SQL Azure. Assistente da base de dados SQL avalia o desempenho das suas bases de dados do SQL Azure, ao analisar o histórico de utilização. Em seguida, oferece recomendações que melhor se adequam para executar a carga de trabalho normal da base de dados. 

> [!NOTE]
> Para obter recomendações, uma base de dados tem de ter sobre uma semana de utilização e, dentro dessa semana deve haver alguma atividade consistente. Assistente da base de dados SQL pode otimizar mais facilmente para padrões de consulta consistente que para extrapolações aleatórias da atividade.

Para obter mais informações sobre o Assistente de base de dados SQL, consulte [Assistente de base de dados SQL](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-redis-cache-performance-and-reliability"></a>Melhorar o desempenho da Cache de Redis e a confiabilidade

Advisor identifica onde desempenho poderá ser afetado negativamente por utilização elevada da memória, de carga do servidor, de largura de banda de rede ou de um grande número de ligações de cliente de instâncias de Cache de Redis. O assistente também fornece as práticas recomendadas recomendações para o ajudar a evitar potenciais problemas. Para obter mais informações sobre as recomendações de Cache de Redis, consulte [Assistente de Cache de Redis](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Melhorar o desempenho do serviço de aplicações e a confiabilidade

O Assistente do Azure integra-se as recomendações de melhores práticas para melhorar a sua experiência de serviços de aplicação e detetar recursos de plataforma relevantes. São exemplos de recomendações dos serviços de aplicações:
* Deteção de instâncias em que a memória ou a recursos da CPU são esgotados por tempos de execução de aplicação com as opções de atenuação.
* Deteção de instâncias onde collocating recursos, como as aplicações web e bases de dados pode melhorar o desempenho e reduzir os custos. 

Para obter mais informações sobre as recomendações de serviços de aplicações, consulte [melhores práticas para o serviço de aplicações do Azure](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Como acessar as recomendações de desempenho no Advisor

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e, em seguida, abra [Advisor](https://aka.ms/azureadvisordashboard).

2.  No dashboard do assistente, clique nas **desempenho** separador.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as recomendações do assistente, consulte:

* [Introdução ao Advisor](advisor-overview.md)
* [Introdução ao Assistente](advisor-get-started.md)
* [Recomendações de custos do Assistente](advisor-performance-recommendations.md)
* [Recomendações de elevada disponibilidade do Assistente](advisor-high-availability-recommendations.md)
* [Recomendações de segurança do Advisor](advisor-security-recommendations.md)

