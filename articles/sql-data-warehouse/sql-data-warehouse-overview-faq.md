---
title: Azure SQL Data Warehouse perguntas mais frequentes | Documentos da Microsoft
description: Este artigo lista as perguntas mais frequentes sobre o Azure SQL Data Warehouse de clientes e os desenvolvedores
services: sql-data-warehouse
author: acomet
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: acomet
ms.reviewer: igorstan
ms.openlocfilehash: 7a570dfa6e0a2812a8dd3a25b8903c70fe07befc
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971708"
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>SQL Data Warehouse perguntas mais frequentes

## <a name="general"></a>Geral

P. O que o SQL DW oferece segurança de dados?

A. O SQL DW oferece várias soluções para proteger os dados, como o TDE e auditoria. Para obter mais informações, consulte [segurança].

P. Onde posso saber quais normas legais ou comerciais é o SQL DW em conformidade com?

A. Visite o [Conformidade da Microsoft] página para várias ofertas de conformidade por produto, como SOC e ISO. Em primeiro lugar selecione por título de conformidade e depois expanda do Azure na Microsoft cloud no âmbito seção serviços no lado direito da página para ver os serviços que estão Azure serviços estão em conformidade.

P. Ligar o Power BI?

A. Sim! Apesar do Power BI suporta a consulta direta com o SQL DW, mas não é adequado para o grande número de utilizadores ou dados em tempo real. Para efeitos de produção do Power BI, recomendamos que utilize o Power BI com base no Azure Analysis Services ou do IaaS do serviço de análise. 

P. Quais são os limites de capacidade de armazém de dados SQL?

A. Consulte nossa atual [limites de capacidade] página. 

P. Por que motivo é meu dimensionamento/colocar em pausa/retomar demorando tanto?

A. Uma variedade de fatores pode influenciar o tempo para operações de gestão de computação. Uma comum caso para operações de longa execução é a reversão transacional. Quando é iniciada uma operação de dimensionamento ou colocar em pausa, todas as sessões de entrada são bloqueadas e consultas são drenadas. Para deixar o sistema num estado estável, as transações tem de ser revertidas volta antes de pode começar uma operação. O maior número e maior o tamanho do registo de transações, será parada mais tempo a operação de restauro do sistema para um estado estável.

## <a name="user-support"></a>Suporte de utilizador

P. Tenho um pedido de funcionalidade, onde submetê-lo?

A. Se tiver um pedido de funcionalidade, submetê-lo no nosso [UserVoice] página

P. Como posso fazer x?

A. Para obter ajuda no desenvolvimento com o SQL Data Warehouse, poderá fazer perguntas nos nossos [Stack Overflow] página. 

P. Como posso submeter um pedido de suporte?

A. [Pedidos de suporte] pode ser arquivado através do portal do Azure.

## <a name="sql-languagefeature-support"></a>Suporte de idioma/funcionalidades do SQL 

P. Que tipos de dados suporta o SQL Data Warehouse?

A. Consulte o SQL Data Warehouse [tipos de dados].

P. Que funcionalidades tabela suporta?

A. Embora o SQL Data Warehouse suporta muitos recursos, alguns não são suportados e estão documentados em [Recursos de tabelas não suportado].

## <a name="tooling-and-administration"></a>As ferramentas e administração

P. Suporta projetos de banco de dados no Visual Studio.

A. Atualmente não suportamos a projetos de banco de dados no Visual Studio para o SQL Data Warehouse. Se gostaria de converter um voto para obter esta funcionalidade, visite nosso User Voice [pedido de funcionalidade de projetos de banco de dados].

P. O SQL Data Warehouse suporta REST APIs?

A. Sim. A maioria das funcionalidades REST que podem ser utilizadas com a base de dados SQL também está disponível com o SQL Data Warehouse. Pode encontrar informações de API em páginas de documentação do REST ou [MSDN].


## <a name="loading"></a>A carregar

P. Que controladores de cliente suporta?

A. Suporte a Driver para o armazém de dados pode ser encontrado no [cadeias de ligação] página

P: quais formatos de ficheiro suportados pelo PolyBase com o SQL Data Warehouse?

R: Orc, o RC, o Parquet e o texto delimitado simples

P: o que posso ligar a partir do armazém de dados SQL com o PolyBase? 

R: [Azure Data Lake Store] e [Blobs de armazenamento do Azure]

P: é possível propagação de computação ao ligar a Blobs de armazenamento do Azure ou do ADLS? 

R: não, o PolyBase do SQL DW interage apenas os componentes de armazenamento. 

P: posso ligar a HDI?

R: HDI pode utilizar o ADLS ou WASB como a camada HDFS. Se tiver como sua camada de HDFS, em seguida, pode carregar esses dados no armazém de dados do SQL. No entanto, não é possível gerar o cálculo de propagação para a instância do HDI. 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o SQL Data Warehouse como um todo, consulte nosso [descrição geral] página.


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Cadeias de Ligação]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Pedidos de suporte]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Segurança]: ./sql-data-warehouse-overview-manage-security.md
[Conformidade da Microsoft]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[limites de capacidade]: ./sql-data-warehouse-service-capacity-limits.md
[tipos de dados]: ./sql-data-warehouse-tables-data-types.md
[Recursos de tabelas não suportado]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Blobs de armazenamento do Azure]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Pedido de funcionalidade de projetos de banco de dados]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/library/azure/mt163685.aspx
[Descrição geral]: ./sql-data-warehouse-overview-faq.md
