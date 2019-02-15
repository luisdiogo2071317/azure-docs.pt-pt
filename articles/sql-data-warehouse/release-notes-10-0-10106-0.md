---
title: Notas de versão do armazém de dados SQL do Azure | Documentos da Microsoft
description: Notas de versão do Azure SQL Data Warehouse.
services: sql-data-warehouse
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.openlocfilehash: 0b1c4c728c23d8bdfe439b3a3db69b06065dad8a
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266956"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Notas de versão do armazém de dados SQL do Azure
Este artigo resume as novas funcionalidades e melhorias nas versões recentes do [do Azure SQL Data Warehouse](sql-data-warehouse-overview-what-is.md). O artigo também apresenta uma lista de atualizações de conteúdo relevantes não diretamente relacionadas com o lançamento, mas publicadas no mesmo intervalo de tempo. Para aprimoramentos a outros serviços do Azure, consulte [as atualizações de serviço](https://azure.microsoft.com/updates)

## <a name="sql-data-warehouse-version-100101060-january"></a>Versão de armazém de dados do SQL 10.0.10106.0 (Janeiro)

### <a name="service-improvements"></a>Melhorias de serviço

| Melhorias de serviço | Detalhes |
| --- | --- |
|**Devolver a ordem pela Otimização**|SELECIONE... ORDER BY consultas obtém um aumento do desempenho nesta versão.   Agora, todos os computação nós enviar que os resultados para um único nó, que une e ordena os resultados, que são devolvidos ao utilizador através do nó de computação de computação.  Intercalar através de um resultados do nó de computação único num ganho de desempenho significativo quando o conjunto de resultados de consulta contém um grande número de linhas. Anteriormente, o motor de execução de consulta seria ordenar os resultados em cada nó de computação e transmiti-los para o nó de controlo, que, em seguida, seria unir os resultados.|
|**Aprimoramentos de movimento de dados para PartitionMove e BroadcastMove**|Técnicas de movimento de dados instantâneas descritas em utilizar o Azure SQL Data Warehouse Gen2, etapas de movimento de dados do tipo ShuffleMove, o [blog de aprimoramentos de desempenho](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/). Com esta versão, os tipos de movimento de dados PartitionMove e BroadcastMove têm agora também a tecnologia as mesmas técnicas de movimento de dados instantânea. Consultas de utilizador que utilizam esses tipos de passos de movimento de dados serão executado com melhoria do desempenho. Nenhuma alteração de código é necessário para aproveitar esses aprimoramentos de desempenho.|

### <a name="documentation-improvements"></a>Melhorias de documentação

| Melhorias de documentação | Detalhes |
| --- | --- |
|nenhum | |
| | |

## <a name="next-steps"></a>Passos Seguintes
- [criar um SQL Data Warehouse](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Mais informações
- [Blogue - Azure SQL Data Warehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Blogues da Equipa Customer Advisory](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Histórias de sucesso de clientes](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Fórum do Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Glossário do Azure](../azure-glossary-cloud-terminology.md)
