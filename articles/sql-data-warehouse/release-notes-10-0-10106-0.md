---
title: Notas de versão de armazém de dados SQL do Azure, Dezembro de 2018 | Documentos da Microsoft
description: Notas de versão do Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 8e82e352ebea4634b1b99864245adcf606352657
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469347"
---
# <a name="whats-new-in-azure-sql-data-warehouse-version---100101060"></a>O que há de novo na versão do Azure SQL Data Warehouse - 10.0.10106.0?
O Azure SQL Data Warehouse (SQL DW) está continuamente a melhorar. Este artigo descreve os novos recursos e alterações que foram introduzidas no armazém de dados do SQL versão 10.0.10106.0.

## <a name="query-restartability---ctas-and-insertselect"></a>Capacidade de reinicialização de consulta - CTAS e inserir/selecionar
Em consultas de raras situações (ou seja, problemas de ligação de rede intermitente, falhas de nó) em execução no Azure SQL DW pode falhar. Já está em execução instruções, como [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) e operações INSERT SELECT, mais são expostos para esse problema potencial. Com esta versão, o Azure SQL DW implementa a lógica de repetição para CTAS e SELECIONE INSERT instruções (além de instruções SELECT anunciadas anteriormente), permitindo que o sistema de forma transparente lidar com esses problemas transitórios e consultas a impedir a falha. O número de tentativas de repetição e a lista de erros transitórios processados são sistema configurado.

## <a name="return-order-by-optimization"></a>Devolver a ordem pela Otimização
SELECIONE... ORDER BY consultas obtém um aumento do desempenho nesta versão.  Anteriormente, o motor de execução de consulta seria ordenar os resultados em cada nó de computação e transmiti-los para o nó de controlo, que, em seguida, seria unir os resultados. Com este melhoramento, todos os de computação em vez disso, nós enviar que os resultados para um único nó, que, em seguida, intercala-los e retorna os resultados ordenados ao utilizador através do nó de computação de computação.  Isso oferece um ganho de desempenho significativo quando o conjunto de resultados de consulta contém um grande número de linhas.

## <a name="data-movement-enhancements-for-partitionmove-and-broadcastmove"></a>Aprimoramentos de movimento de dados para PartitionMove e BroadcastMove
No Azure SQL Data Warehouse Gen2, etapas de movimento de dados do tipo ShuffleMove tirar partido das técnicas de movimento de dados instantâneas descritas os [blog de aprimoramentos de desempenho aqui](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/).  Com esta versão, os tipos de movimento de dados PartitionMove e BroadcastMove têm agora também a tecnologia as mesmas técnicas de movimento de dados instantânea.  Consultas de utilizador que utilizam esses tipos de passos de movimento de dados irão ver um aumento do desempenho.  Nenhuma alteração de código é necessário para aproveitar esses ganhos de desempenho.

## <a name="next-steps"></a>Passos Seguintes
Agora que já sabe um pouco sobre o SQL Data Warehouse, saiba como rapidamente [criar um SQL Data Warehouse][create a SQL Data Warehouse]. Se estiver familiarizado com o Azure, pode achar a [Glossário do Azure] [ Azure glossary] útil medida que aprende a terminologia nova. Em alternativa, veja alguns destes outros Recursos do SQL Data Warehouse.  

* [Histórias de sucesso de clientes]
* [Blogues]
* [Pedidos de funcionalidades]
* [Vídeos]
* [Blogues da Equipa Customer Advisory]
* [Fórum do Stack Overflow]
* [Twitter]


[Blogues]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogues da Equipa Customer Advisory]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Histórias de sucesso de clientes]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Pedidos de funcionalidades]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum do Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Vídeos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
