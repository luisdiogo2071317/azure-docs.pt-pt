---
title: Notas de versão de armazém de dados SQL do Azure, Junho de 2018 | Documentos da Microsoft
description: Notas de versão do Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 86aadcbdd8d7168440726d6dbed996629cad3ff7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285338"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>O que há de novo no Azure SQL Data Warehouse? Julho de 2018
O Azure SQL Data Warehouse recebe melhorias continuamente. Este artigo descreve os novos recursos e alterações que foram introduzidas em Julho de 2018.


## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Granularidade mais fina para cruzada restaurações de região e o servidor
Agora, pode restaurar em regiões e servidores através de qualquer ponto de restauro em vez de selecionar geo redundante cópias de segurança que são executadas a cada 24 horas. Cruzada restauro de região e servidor são suportadas para ambos os pontos de restauração de automática pelo utilizador ou ativar a granularidade mais fina para proteção de dados adicionais. Com mais pontos de restauro disponíveis, pode ter a garantia de que o armazém de dados será logicamente consistente durante a restauração entre regiões.

![Restaurar Command - Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![opções de restauro - Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Para obter mais informações, consulte a [Accelerated e pontos de restauro flexível](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) postagem de blog.

## <a name="20-minute-restorations"></a>Restaurações de 20 minutos
SQL Data Warehouse oferece agora o restauro de qualquer armazém de dados estiver **menos de 20 minutos** dentro da mesma região, independentemente do tamanho do banco de dados. O tempo de restauro aplica-se quer o restauro estiver no mesmo ou um servidor lógico diferente na mesma região. Além disso, o processo de instantâneo foi aperfeiçoado para reduzir a quantidade de tempo necessário para criar um ponto de restauro. Em lower níveis de desempenho (definições de DWU inferior), o aprimoramento é um *redução de 2x* para a criação do instantâneo no tempo.

Para obter mais informações, consulte a [Accelerated e pontos de restauro flexível](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) postagem de blog.

## <a name="custom-restoration-configurations"></a>Configurações de restauração personalizados
Agora pode alterar o nível de desempenho (DWU) ao restaurar no portal do Azure. Também pode restaurar um armazém de dados de geração 2 atualizado. Ao restaurar com uma instância de Gen 2, agora pode avaliar o impacto de geração 2 antes [atualizar o seu armazém de dados de geração 1](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Configuração de restauração personalizados – armazém de dados SQL do Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
O [sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) procedimento armazenado, muitas vezes, é utilizado pelas ferramentas para obter metadados sobre parâmetros no batch do Transact-SQL. O procedimento retorna uma linha para cada parâmetro no batch com as informações de tipo deduzido para esse parâmetro. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

O conjunto de resultados inclui metadados sobre o `@id` parâmetro (resultados parciais mostrados)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```

## <a name="next-steps"></a>Passos Seguintes
Agora que já sabe um pouco sobre o SQL Data Warehouse, saiba como rapidamente [criar um SQL Data Warehouse] [criar um SQL Data Warehouse] e [carregar dados de exemplo] [carregar dados de exemplo]. Se estiver familiarizado com o Azure, pode achar [Glossário do Azure] [Glossário do Azure] útil à medida que encontra terminologia nova. Em alternativa, veja alguns destes outros Recursos do SQL Data Warehouse.  

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
