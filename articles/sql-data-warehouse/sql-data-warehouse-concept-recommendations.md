---
title: Recomendações - conceitos do SQL Data Warehouse | Documentos da Microsoft
description: Saiba mais sobre as recomendações de SQL Data Warehouse e como eles são gerados
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 48d64873f0a8c3754ac5c3ecda2294c0f337b9d5
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094609"
---
# <a name="sql-data-warehouse-recommendations"></a>Recomendações de armazém de dados SQL

Este artigo descreve as recomendações fornecidas ao SQL Data Warehouse através do Assistente do Azure.  

O SQL Data Warehouse fornece recomendações para garantir que o seu armazém de dados consistentemente está otimizado para desempenho. Recomendações de armazém de dados são totalmente integradas com [do Assistente do Azure](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) para lhe fornecer melhores práticas diretamente dentro do [portal do Azure](https://aka.ms/Azureadvisor). O SQL Data Warehouse analisa o estado atual do seu armazém de dados, recolhe telemetria e recomendações de superfícies sobre a sua carga de trabalho ativada diariamente. Os cenários de recomendação do armazém de dados suportados estão descritos a seguir, juntamente com a aplicar as ações recomendadas.

Se tiver comentários sobre o Assistente de armazém de dados SQL ou se depare com problemas, contacte [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Clique em [aqui](https://aka.ms/Azureadvisor) para verificar as suas recomendações hoje mesmo! Atualmente esta funcionalidade é aplicável a armazéns de dados de geração 2 apenas. 

## <a name="data-skew"></a>Distorção de dados

Distorção de dados pode provocar afunilamentos de movimento ou recurso de dados adicionais ao executar a sua carga de trabalho. A seguinte documentação descreve show para identificar a distorção de dados e impedir que ele aconteça, selecionando uma chave de distribuição ideal.

- [Identificar e remover inclinação](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Não ou desatualizados estatísticas

Com estatísticas inferior ao ideal pode afetar gravemente o desempenho das consultas que pode fazer com que o otimizador de consultas do SQL Data Warehouse gerar planos de consulta inferior ao ideal. A seguinte documentação descreve as práticas recomendadas sobre a criação e atualização de estatísticas de:

- [Criar e atualizar estatísticas de tabela](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Para ver a lista de tabelas afetadas por estas recomendações, execute o seguinte [script T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables). O assistente executa continuamente o mesmo script T-SQL para gerar estas recomendações.
