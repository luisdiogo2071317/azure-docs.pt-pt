---
title: Consulta elástica - aceder a dados no Azure SQL Data Warehouse da SQL Database do Azure | Documentos da Microsoft
description: Aprenda as melhores práticas para a utilização de consulta elástica da base de dados do Azure SQL para aceder a dados no Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/11/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: d861e1d4cd891e1f1e1be3209ae4dfdbf4420165
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44718306"
---
# <a name="best-practices-for-using-elastic-query-in-azure-sql-database-to-access-data-in-azure-sql-data-warehouse"></a>Melhores práticas para utilizar a consulta elástica na base de dados do Azure SQL para aceder a dados no Azure SQL Data Warehouse
Aprenda as melhores práticas para a utilização de consulta elástica da base de dados do Azure SQL para aceder a dados no Azure SQL Data Warehouse. 

## <a name="what-is-an-elastic-query"></a>O que é uma consulta elástica?
Uma consulta elástica permite-lhe utilizar o T-SQL e tabelas externas para escrever uma consulta numa base de dados SQL do Azure que é enviada remotamente para um armazém de dados SQL do Azure. Ao utilizar esta funcionalidade fornece a economia de custos e mais arquiteturas de alto desempenho, dependendo do cenário.

Esta funcionalidade permite que os dois cenários principais:

1. Isolamento de domínio
2. Execução de consulta remota

### <a name="domain-isolation"></a>Isolamento de domínio

Isolamento de domínio refere-se para o cenário de mart dados clássico. Em determinados cenários, um talvez queira fornecer um domínio de lógico de dados aos utilizadores downstream isolados do restante do armazém de dados, por uma série de motivos, incluindo, mas não se limitando a:

1. Isolamento de recursos - base de dados SQL é otimizado para servir uma grande base de utilizadores em simultâneo que servem as cargas de trabalho ligeiramente diferentes do que as consultas de análise de grandes que o armazém de dados está reservado para. Isolamento garante que as cargas de trabalho direito são servidas pelas ferramentas certas.
2. Isolamento de segurança - separar um subconjunto de dados autorizados seletivamente por meio de determinados esquemas.
3. Áreas de segurança - fornecer um conjunto de dados de exemplo como um "recreio" para explorar as consultas de produção etc.

Consulta elástica pode fornecer a capacidade de facilmente selecionar subconjuntos de dados de armazém de dados SQL e mova-o para uma instância de base de dados SQL. Além disso, esse isolamento não impedir a capacidade de ativar também a execução de consulta remota, permite cenários de "cache" mais interessantes.

### <a name="remote-query-execution"></a>Execução de consulta remota

Consulta elástica permite a execução da consulta remota numa instância de armazém de dados SQL. Um pode utilizar o melhor da base de dados SQL e SQL data warehouse, separando os seus dados frequentes e esporádica entre duas bases de dados. Os usuários podem manter os dados mais recentes numa base de dados SQL, que pode servir um grande número de utilizadores de empresas de médio e de relatórios. No entanto, quando for necessário mais dados ou computação, um utilizador pode descarregar a parte da consulta para uma instância de armazém de dados SQL, onde podem ser processadas em grande escala agregações muito mais rapidamente e com mais eficiência.

## <a name="elastic-query-process"></a>Processo de consulta elástica
Uma consulta elástica pode ser utilizada para disponibilizar dados localizados dentro de um SQL data warehouse para instâncias de base de dados SQL. Consulta elástica permite consultas a partir de uma base de dados SQL, consulte a tabelas numa instância de armazém de dados remota do SQL. 

A primeira etapa é criar uma definição de origem de dados externos que refere-se para a instância de armazém de dados SQL, que utiliza as credenciais de utilizador existente no SQL data warehouse. Nenhuma alteração é necessária na instância de armazém de dados do SQL remota. 

> [!IMPORTANT] 
> 
> Precisa de ter permissão de alterar qualquer origem de dados externa. Esta permissão está incluída com a permissão ALTER DATABASE. Para fazer referência a origens de dados remoto, são necessárias permissões de alterar qualquer origem de dados externa.

Em seguida, crie uma definição de tabela externa remoto numa instância de base de dados SQL, que aponta para uma tabela remota no armazém de dados SQL. Quando uma consulta utiliza uma tabela externa, a parte da consulta que faça referência a tabela externa é enviada para a instância de armazém de dados SQL a ser processado. Depois de concluída a consulta, o conjunto de resultados é enviado para a instância de base de dados SQL chamada. Para um breve tutorial de configuração de uma consulta elástica entre a base de dados SQL e SQL data warehouse, consulte a [configurar consulta elástica com o SQL Data Warehouse][Configure Elastic Query with SQL Data Warehouse].

Para obter mais informações sobre a consulta elástica de base de dados SQL, consulte a [descrição geral de consulta elástica de base de dados do Azure SQL][Azure SQL Database elastic query overview].

## <a name="best-practices"></a>Melhores práticas
Utilize estas melhores práticas para utilizar a consulta elástica com eficiência.

### <a name="general"></a>Geral

- Ao utilizar a execução da consulta remota, certifique-se de que está apenas selecionar as colunas necessárias e aplicar os filtros corretos. Não apenas faz esse aumento de computação necessário, mas também aumenta o tamanho do conjunto de resultados e, portanto, a quantidade de dados que precisam de ser movidos entre as duas instâncias.
- Manter os dados para fins analíticos no SQL Data Warehouse e base de dados SQL no columnstore em cluster para um desempenho analytiIcal.
- Certifique-se de que as tabelas de origem são particionadas para movimento de dados e de consultas.
- Certifique-se de que as instâncias do SQL da base de dados usadas como cache são particionadas para permitir atualizações mais granulares e uma gestão mais fácil. 
- O ideal é que utilize bases de dados do PremiumRS porque fornecem os benefícios analíticos de indexação com foco em cargas de trabalho de e/s intensivas com um desconto de bases de dados Premium de columnstore em cluster.
- Depois de cargas, utilize colunas de identificação de data efetiva para upserts nas instâncias de base de dados SQL para manter a integridade de origem de cache ou de carga. 
- Crie um início de sessão separado e um utilizador na sua instância do armazém de dados SQL para as credenciais de início de sessão remoto do SQL da base de dados definidas na origem de dados externa. 

### <a name="elastic-querying"></a>Consulta elástica

- Em muitos casos, um poderá querer gerir um tipo de tabela Stretch, em que é uma parte da sua tabela na base de dados SQL que os dados em cache o desempenho com o resto dos dados armazenados no SQL Data Warehouse. Precisa de dois objetos na base de dados SQL: uma tabela externa na base de dados do SQL que referencia a tabela base no SQL Data Warehouse e a parte "em cache" da tabela na base de dados SQL. Considere criar uma vista ao longo da parte superior da parte em cache da tabela e a tabela externa que ambas as tabelas e aplica-se de filtros que separam dados materializados na base de dados SQL e SQL Data Warehouse dados expostos por meio de tabelas externas.

  Imagine que pretende manter o ano mais recente dos dados numa instância de base de dados SQL. O **ext. Pedidos** referências de tabela orders de armazém de dados de tabelas. O **dbo. Pedidos** representa o valor mais recente de anos de dados dentro da instância de base de dados SQL. Em vez de pedir aos utilizadores para decidir se pretende consultar uma tabela ou outro, crie uma vista ao longo da parte superior das duas tabelas no ponto de partição do ano mais recente.

  ```sql
  CREATE VIEW dbo.Orders_Elastic AS
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]
  FROM
    [dbo].[Orders]
  WHERE 
    YEAR([o_orderdate]) >= '<Most Recent Year>'
  UNION
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]         
  FROM
    [ext].[Orders]
  WHERE
    YEAR([o_orderdate]) < '<Most Recent Year>'
  ```

  Uma vista produzidos de maneira vamos o compilador de consulta determinar se necessita de utilizar a instância de armazém de dados para responder à consulta de utilizadores. 

  Há sobrecarga de envio, compilar, executar e mover os dados associados a cada consulta elástica em relação a instância de armazém de dados. Ter conhecimento de que cada consulta elástica é deduzida da sua ranhuras de simultaneidade e utiliza recursos.  


- Se um planos desagregar mais detalhadamente o conjunto de resultados a partir da instância de armazém de dados, considere materializá-lo numa tabela temporária na base de dados SQL para o desempenho e para impedir a utilização de recursos desnecessários.

### <a name="moving-data"></a>Mover dados 

- Se possível, mantenha a gestão de dados mais fácil com as tabelas de origem só de acréscimo, de modo a que as atualizações são facilmente passível de manutenção entre as instâncias de base de dados e de armazém de dados.
- Mover dados no nível da partição com remoção da cache e semântica de preenchimento para minimizar o custo de consultas nos dados do armazém de nível e a quantidade de dados movidos para manter a instância de base de dados atualizados. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Quando escolher a base de dados SQL do Azure Analysis Services vs

Utilizar o Azure Analysis Services quando:

- Tencione utilizar a sua cache com uma ferramenta de BI que envia um grande número de consultas pequeno
- Precisa subsecond latência da consulta
- Têm experiência no gerenciamento/desenvolvimento de modelos para o Analysis Services 

Utilizar o Azure SQL da base de dados quando:

- Que pretende consultar dados da cache com o SQL
- Terá de execução remota para determinadas consultas
- Tiver maiores requisitos de cache

## <a name="faq"></a>FAQ

P: Posso utilizar bases de dados dentro de um conjunto elástico com consulta elástica?

R: Sim. Bases de dados SQL num conjunto elástico que pode utilizar a consulta elástica. 

P: existe um limite para quantas bases de dados que pode utilizar para a consulta elástica?

R: não existe nenhum limite de máximo de disco rígido em quantos bancos de dados podem ser utilizados para a consulta elástica. No entanto, cada consulta elástica (consultas que atingem o SQL Data Warehouse) serão revertidas em limites de simultaneidade normal.

P: existem limites DTU envolvidas com a consulta elástica?

R: limites de DTU de não são impostas qualquer modo diferente com a consulta elástica. A política padrão é, de modo a que os servidores lógicos têm limites DTU em vigor para impedir que os clientes ultrapassar o limite acidental. Se pretende ativar a vários bancos de dados para a consulta elástica juntamente com uma instância do SQL Data Warehouse, pode atingir o limite máximo de inesperadamente. Se isto ocorrer, submeta um pedido para aumentar o limite DTU no seu servidor lógico. Pode aumentar a quota por [criar um pedido de suporte](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) e selecionando *Quota* como o tipo de pedido

P: Posso utilizar dados de linha nível segurança/dinâmico máscara com consulta elástica?

R: clientes que pretendem utilizar recursos de segurança mais avançados com base de dados SQL podem fazê-lo, primeiro mover e armazenar os dados na base de dados SQL. Atualmente não é possível aplicar a segurança ao nível da linha ou de ddm de autor em dados consultados por meio de tabelas externas. 

P: posso escrever da minha instância de base de dados SQL para a instância do armazém de dados?

R: atualmente esta funcionalidade não é suportada. Visite nosso [página de comentários] [ Feedback page] para criar/um voto para esta funcionalidade se se tratar de uma funcionalidade que gostaria de ver no futuro. 

P: Posso utilizar tipos geográficos como geometria/geografia?

R: pode armazenar tipos espaciais no SQL Data Warehouse como valores de varbinary (Max). Quando consultar estas colunas com uma consulta elástica, pode convertê-los para os tipos apropriados em tempo de execução.

![tipos geográficos](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)

<!--Article references-->

[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Configure Elastic Query with SQL Data Warehouse]: tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md


