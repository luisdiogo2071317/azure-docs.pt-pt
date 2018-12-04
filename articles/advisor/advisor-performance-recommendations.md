---
title: Recomendações de desempenho do Assistente do Azure | Documentos da Microsoft
description: Utilize o Assistente para otimizar o desempenho das suas implementações do Azure.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: ba79d2d5ed4350960af1b92ee863595e3540a1d2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843932"
---
# <a name="advisor-performance-recommendations"></a>Recomendações de desempenho do Assistente

Recomendações de desempenho do Assistente do Azure ajudam a melhorar a velocidade e a capacidade de resposta das suas aplicações críticas para a empresa. Pode obter recomendações de desempenho do Advisor o **desempenho** separador do dashboard do Advisor.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Reduzir o tempo DNS ao vivo no seu perfil do Gestor de tráfego para a ativação pós-falha para pontos finais de bom estado de funcionamento mais rapidamente

[Tempo para as definições de Live (TTL)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) no perfil do Traffic Manager permitem-lhe especificar como rapidamente mudar pontos de extremidade, se um determinado ponto de final deixa de responder às consultas. Reduzir os valores TTL significa que os clientes serão encaminhados para pontos finais de funcionamento mais rápidos.

O Azure Advisor identifica perfis do Gestor de tráfego com um valor de TTL mais configurado e recomenda configurar o valor de TTL para 20 segundos ou 60 segundos, dependendo se o perfil está configurado para [rápida ativação pós-falha](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>Melhore o desempenho da base de dados com o Assistente da BD SQL

O assistente fornece-lhe uma vista consolidada consistente de recomendações para todos os seus recursos do Azure. Ele se integra com o Assistente de base de dados SQL para lhe oferecer recomendações para melhorar o desempenho da base de dados do SQL Azure. Assistente da base de dados SQL avalia o desempenho das suas bases de dados do SQL Azure, ao analisar o histórico de utilização. Em seguida, oferece recomendações que melhor se adequam para executar a carga de trabalho normal da base de dados. 

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

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Remover distorção na sua tabela de armazém de dados SQL para melhorar o desempenho de consulta de dados

Distorção de dados pode provocar afunilamentos de movimento ou recurso de dados desnecessários ao executar a sua carga de trabalho. O assistente Deteta os dados de distribuição inclinar superiores a 15% e recomendável redistribuir os seus dados e examine as seleções de chave de distribuição de tabela. Para saber mais sobre identificar e remover distorção, veja [resolução de problemas distorção](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Criar ou atualizar estatísticas de tabela Desatualizadas em sua tabela de armazém de dados SQL para melhorar o desempenho de consulta

Advisor identifica as tabelas que não tenham atualizado [estatísticas de tabela](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) e recomenda criar ou atualizar estatísticas de tabela. Consulta otimizador utiliza ukazatelé atualizado para estimar a cardinalidade ou o número de linhas no resultado da consulta que permite que o otimizador de consultas criar um plano de consulta de alta qualidade para um desempenho mais rápido do armazém de dados do SQL.

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Migrar a sua conta de armazenamento para o Azure Resource Manager para obter todas as funcionalidades do Azure mais recente

Migre o seu modelo de implementação da conta de armazenamento para Azure Resource Manager (ARM) para tirar partido das implementações de modelo, opções de segurança adicionais e a capacidade de atualizar para uma conta GPv2 para utilização de funcionalidades mais recentes do armazenamento do Azure. O assistente irá identificar quaisquer contas de armazenamento autónomo que estão a utilizar o modelo de implementação clássica e recomenda a migração para o modelo de implementação ARM. 

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

