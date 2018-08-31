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
ms.openlocfilehash: 35ae3d1a8a6de2d348f90e2f55b732421b879917
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307642"
---
# <a name="sql-data-warehouse-recommendations"></a>Recomendações de armazém de dados SQL

Este artigo descreve as recomendações fornecidas ao SQL Data Warehouse através do Assistente do Azure.  

O SQL Data Warehouse fornece recomendações para garantir que o seu armazém de dados consistentemente está otimizado para desempenho. Recomendações de armazém de dados está estreitamente integrado com [do Assistente do Azure](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) para lhe fornecer melhores práticas diretamente dentro do [portal do Azure](https://aka.ms/Azureadvisor). O SQL Data Warehouse analisa o estado atual do seu armazém de dados, recolhe telemetria e recomendações de superfícies sobre a sua carga de trabalho ativada diariamente. Os cenários de recomendação do armazém de dados suportados estão descritos a seguir, juntamente com a aplicar as ações recomendadas.

Se tiver comentários sobre o Assistente de armazém de dados SQL ou se depare com problemas, contacte [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Clique em [aqui](https://aka.ms/Azureadvisor) para verificar as suas recomendações hoje mesmo! Atualmente esta funcionalidade é aplicável a armazéns de dados de geração 2 apenas. 

## <a name="data-skew"></a>Distorção de dados

Distorção de dados pode provocar afunilamentos de movimento ou recurso de dados adicionais ao executar a sua carga de trabalho. A seguinte documentação descreve show para identificar a distorção de dados e impedir que ele aconteça, selecionando uma chave de distribuição ideal.

- [Identificar e remover inclinação](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Não ou desatualizados estatísticas

Com estatísticas inferior ao ideal pode afetar gravemente o desempenho das consultas que pode fazer com que o otimizador de consultas do SQL Data Warehouse gerar planos de consulta inferior ao ideal. A seguinte documentação descreve as práticas recomendadas sobre a criação e atualização de estatísticas de:

- [Criar e atualizar estatísticas de tabela](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Para estas dois recomendações, o advisor está continuamente a executar o seguinte [script T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) para identificar afetadas por recomendações skew e estatísticas de tabelas.
