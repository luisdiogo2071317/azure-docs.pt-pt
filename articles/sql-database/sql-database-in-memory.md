---
title: Tecnologias de SQL da base de dados dentro da memória do Azure | Documentos da Microsoft
description: Tecnologias de SQL da base de dados dentro da memória do Azure aumentam muito o desempenho do transacional e cargas de trabalho de análise.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jodebrui
ms.author: jodebrui
ms.reviewer: ''
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: 8fd430aa710c7e36133b40c7079b9d727774c68f
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166933"
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Otimizar o desempenho ao utilizar tecnologias dentro da memória na base de dados SQL

Ao utilizar tecnologias dentro da memória na base de dados do Azure SQL, pode conseguir melhorias de desempenho com várias cargas de trabalho: (online transacional processamento transacional (OLTP)), análise (processamento analítico online (OLAP)) e misto (transação híbrida / analítico de processamento (HTAP)). Devido à consulta mais eficiente e processamento de transações, tecnologias dentro da memória também ajudá-lo a reduzir os custos. Normalmente, não terá de atualizar o escalão de preço da base de dados para atingir ganhos de desempenho. Em alguns casos, pode até mesmo ser capazes de reduzir o escalão de preço, enquanto ainda veem melhorias de desempenho com tecnologias dentro da memória.

Aqui estão dois exemplos de como o OLTP dentro da memória ajudou a melhorar significativamente o desempenho:

- Com o OLTP dentro da memória, [soluções de negócios de quórum foi capaz de duplicar a sua carga de trabalho enquanto melhora DTUs 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU significa *unidade de transação de base de dados*, e inclui uma medida de consumo de recursos.
- O vídeo seguinte demonstra uma melhoria significativa no consumo de recursos com uma carga de trabalho de exemplo: [OLTP dentro da memória na base de dados SQL do Azure vídeo](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Para obter mais informações, consulte a mensagem de blogue: [OLTP dentro da memória na mensagem de blogue do Azure SQL da base de dados](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

Tecnologias dentro da memória estão disponíveis em todas as bases de dados no escalão Premium, incluindo bases de dados em conjuntos elásticos Premium.

O vídeo seguinte explica os ganhos de desempenho possíveis com tecnologias dentro da memória na base de dados do Azure SQL. Lembre-se de que o ganho de desempenho que verá sempre depende de vários fatores, incluindo a natureza da carga de trabalho e os dados, o padrão de acesso da base de dados e assim por diante.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Base de dados SQL do Azure tem as seguintes tecnologias dentro da memória:

- *OLTP dentro da memória* aumenta a transação e reduz a latência para processamento de transações. Os cenários que tiram partido de OLTP dentro da memória são: processamento, como de negociação e de jogos, ingestão de dados de eventos ou dispositivos de IoT, colocação em cache, o carregamento de dados e a tabela temporária e cenários de variável de tabela de transações de alto débito.
- *Colocar em cluster os índices columnstore* reduzir sua pegada de armazenamento (até 10 vezes) e melhore o desempenho de consultas de análise e relatórios. Pode utilizá-lo com tabelas de fatos em seus data marts para se ajustar mais dados na base de dados e melhorar o desempenho. Além disso, pode utilizá-lo com dados históricos na sua base de dados operacional para arquivar e ser capaz de consultar até 10 vezes mais dados.
- *Os índices columnstore não em cluster* para HTAP ajudam-para obter informações em tempo real sobre o seu negócio através de consultar a base de dados operacional diretamente, sem a necessidade de executar uma extração cara, transformar e carregar (ETL) processar e espere até que o armazém de dados deve ser preenchida. Os índices columnstore não em cluster permitem muito rápida execução das consultas de análise na base de dados OLTP, reduzindo o impacto sobre a carga de trabalho operacional.
- Também pode ter a combinação de uma tabela com otimização de memória com um índice columnstore. Esta combinação permite-lhe efetuar o processamento de transações muito rápida e, a *simultaneamente* executar consultas de análise muito rapidamente nos mesmos dados.

Os índices columnstore e OLTP dentro da memória desde parte do produto SQL Server 2012 e de 2014, respectivamente. Base de dados SQL do Azure e o SQL Server compartilham a mesma implementação de tecnologias dentro da memória. Daqui em diante, novas capacidades para essas tecnologias são lançadas na base de dados do Azure SQL em primeiro lugar, antes de serem liberadas no SQL Server.

Este artigo descreve aspectos do OLTP dentro da memória e columnstore índices que são específicos para a base de dados do Azure SQL e também inclui exemplos:
- Verá o impacto dessas tecnologias em limites de tamanho de armazenamento e dados.
- Verá como gerir o movimento de bases de dados usar essas tecnologias entre os escalões de preços diferentes.
- Verá dois exemplos que ilustram o uso de OLTP na memória, bem como os índices columnstore na base de dados do Azure SQL.

Consulte os seguintes recursos para obter mais informações.

Obter informações aprofundadas sobre as tecnologias:

- [Descrição geral de OLTP na memória e cenários de uso](https://msdn.microsoft.com/library/mt774593.aspx) (inclui referências a casos práticos de clientes e informações para começar a trabalhar)
- [Documentação para OLTP dentro da memória](http://msdn.microsoft.com/library/dn133186.aspx)
- [Guia de índices Columnstore](https://msdn.microsoft.com/library/gg492088.aspx)
- Híbrido transacional/analítico de processamento (HTAP), também conhecido como [análises operacionais em tempo real](https://msdn.microsoft.com/library/dn817827.aspx)

Uma palavra em OLTP na memória: [1 de início rápido: tecnologias de OLTP dentro da memória para um desempenho de T-SQL com mais rapidez](http://msdn.microsoft.com/library/mt694156.aspx) (a outro artigo para ajudá-lo a começar)

Vídeos detalhados sobre as tecnologias:

- [OLTP dentro da memória na base de dados do Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (que contém uma demonstração dos benefícios de desempenho e passos para reproduzir esses resultados por conta própria)
- [Vídeos OLTP na memória: O que é e quando/como usá-lo](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [O índice Columnstore: Vídeos de análise dentro da memória do Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Tamanho de armazenamento e dados

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Limite de tamanho e o armazenamento de dados para OLTP dentro da memória

OLTP dentro da memória inclui tabelas com otimização de memória, que são utilizadas para armazenar dados de utilizador. Essas tabelas são necessários para caber na memória. Como gerencia a memória diretamente no serviço de base de dados SQL, temos o conceito de uma quota para dados de utilizador. Essa idéia é referida como *armazenamento OLTP dentro da memória*.

Cada base de dados única suportado e cada conjunto elástico escalão de preço do escalão de preço inclui uma certa quantidade de armazenamento OLTP dentro da memória. Ver [limites de recursos baseados em DTU - base de dados individual](sql-database-dtu-resource-limits-single-databases.md), [limites de recursos baseados em DTU - conjuntos elásticos](sql-database-dtu-resource-limits-elastic-pools.md),[limites de recursos baseados em vCore - bases de dados individuais](sql-database-vcore-resource-limits-single-databases.md) e [limites de recursos baseados em vCore - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).

Os seguintes itens contam para o limite de armazenamento OLTP dentro da memória:

- Linhas de dados de utilizador ativo em tabelas com otimização de memória e as variáveis de tabela. Tenha em atenção que as versões de linha antiga não contam para o limite máximo.
- Índices em tabelas com otimização de memória.
- Sobrecarga operacional de operações de ALTER TABLE.

Se atingir o limite, receberá um erro de limite de quota e já não é possível inserir ou atualizar os dados. Para mitigar este erro, eliminar dados ou aumentar o escalão de preço da base de dados ou do conjunto.

Para obter detalhes sobre como monitorizar a utilização do armazenamento OLTP dentro da memória e configurar alertas quando quase a atingir o limite, consulte [armazenamento de monitorizar dentro da memória](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Sobre conjuntos elásticos

Com os conjuntos elásticos, o armazenamento OLTP dentro da memória é partilhado entre todas as bases de dados no conjunto. Por conseguinte, a utilização numa base de dados pode potencialmente afetar outras bases de dados. São dois mitigações para isso:

- Configurar uma `Max-eDTU` ou `MaxvCore` para bases de dados que é menor do que a contagem de eDTU ou vCore para o conjunto como um todo. Este valor máximo caps a utilização de armazenamento OLTP dentro da memória, em qualquer base de dados no conjunto, o tamanho que corresponde à contagem de eDTU.
- Configurar uma `Min-eDTU` ou `MinvCore` superior a 0. Este mínimo garante que cada base de dados no conjunto tem a quantidade de armazenamento OLTP dentro da memória disponível que corresponde à configurada `Min-eDTU` ou `vCore`.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Tamanho dos dados e armazenamento para os índices columnstore

Os índices Columnstore não são necessárias para caber na memória. Por conseguinte, o único limite no tamanho dos índices é o máximo da base de dados tamanho geral, que está documentado na [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) artigos.

Quando utiliza os índices columnstore em cluster, é utilizada compressão em colunas para o armazenamento de tabela base. Esta compressão pode reduzir significativamente os requisitos de espaço de armazenamento dos seus dados de utilizador, o que significa que pode se encaixar mais dados na base de dados. E a compactação é possível aumentar ainda mais ao [compactação de arquivo em colunas](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). A quantidade de compressão que pode alcançar depende da natureza dos dados, mas 10 vezes a compressão não é incomum.

Por exemplo, se tiver uma base de dados com um tamanho máximo de 1 terabyte (TB) e obter a compactação de 10 vezes, utilizando os índices columnstore, pode se encaixar um total de 10 TB de dados de utilizador na base de dados.

Quando utiliza os índices columnstore não em cluster, a tabela base ainda é armazenada no formato rowstore tradicional. Por conseguinte, a economia de armazenamento não é tão grande como com índices columnstore em cluster. No entanto, se a substituição de um número de índices não agrupados tradicionais com um índice columnstore único, ainda poderá ver uma redução geral nos requisitos de espaço de armazenamento para a tabela.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Mover bases de dados que utilizam tecnologias dentro da memória entre os escalões de preços

Nunca haverá qualquer incompatibilidades ou outros problemas ao atualizar para um escalão de preço superior, como de Standard para Premium. Apenas aumentam os recursos e funcionalidade disponível.

Mas fazer downgrade o escalão de preço pode afetar negativamente o seu banco de dados. O impacto é especialmente evidente quando para mudar do Premium para Standard ou Basic quando a base de dados contém objetos de OLTP na memória. Tabelas com otimização de memória não estão disponíveis após a mudança para versão anterior (mesmo que eles permanecem visíveis). As mesmas considerações aplicam-se quando está reduzindo o escalão de preço de um conjunto elástico ou mover uma base de dados com tecnologias dentro da memória, para um Standard ou Basic conjunto elástico.

### <a name="in-memory-oltp"></a>OLTP dentro da memória

*Fazendo downgrade para básico/Standard*: OLTP dentro da memória não é suportado em bases de dados no escalão Standard ou Basic. Além disso, não é possível mover uma base de dados com quaisquer objetos de OLTP na memória para o escalão Standard ou Basic.

Existe uma forma programática para compreender se um determinado banco de dados suporta o OLTP dentro da memória. Pode executar a consulta de Transact-SQL seguinte:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Se a consulta devolve **1**, OLTP dentro da memória é suportado nesta base de dados.

Antes de mudar a versão da base de dados para o Standard/Basic, remova todas as tabelas com otimização de memória e tipos de tabela, bem como todos os módulos de T-SQL compilados nativamente. As seguintes consultas de identificam todos os objetos que tenham de ser removidas antes de uma base de dados pode ser desatualizada para básico/Standard:

```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

*Fazendo downgrade para um escalão mais baixo do Premium*: dados em tabelas com otimização de memória devem caber no armazenamento OLTP dentro da memória que está associado com o escalão de preço da base de dados ou está disponível no conjunto elástico. Se tentar reduzir o escalão de preço ou mover a base de dados para um conjunto que não tem armazenamento OLTP dentro da memória suficiente disponível, a operação falhar.

### <a name="columnstore-indexes"></a>Índices Columnstore

*Fazendo downgrade para básico ou Standard*: índices Columnstore são suportados apenas no escalão de preços Premium e no escalão Standard S3 e acima e não no escalão básico. Quando mudar a sua base de dados para um escalão não suportado ou o nível de versão, o índice columnstore fica indisponível. O sistema mantém o índice columnstore, mas nunca utiliza o índice. Se atualizar mais tarde para uma camada de suporte ou o nível, o índice columnstore é imediatamente pronto para ser aproveitadas novamente.

Se tiver um **em cluster** índice columnstore, a tabela inteira fica indisponível após a mudança para versão anterior. Por conseguinte, recomendamos que remova todos *em cluster* columnstore indexa antes de mudar a versão de base de dados para um nível ou camada não suportada.

*Fazendo downgrade para um escalão mais baixo de suporte ou o nível*: Esta mudança para versão anterior tiver êxito, se todo o banco de dados se encaixa no tamanho máximo da base de dados para o escalão de preço de destino ou dentro do armazenamento disponível no conjunto elástico. Não há nenhum impacto específico dos índices columnstore.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Instalar o exemplo de OLTP na memória

É possível criar a base de dados de exemplo do AdventureWorksLT com alguns cliques na [portal do Azure](https://portal.azure.com/). Em seguida, os passos nesta secção explicam como pode enriquecer a sua base de dados AdventureWorksLT com objetos de OLTP na memória e demonstrar os benefícios de desempenho.

Para uma mais simples, mas visualmente mais atraente desempenho demonstração para OLTP dentro da memória, consulte:

- Versão: [no-memória-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- O código-fonte: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Passos de instalação

1. Na [portal do Azure](https://portal.azure.com/), criar uma base de dados Premium ou críticas para a empresa num servidor. Definir o **origem** na base de dados de exemplo do AdventureWorksLT. Para obter instruções detalhadas, consulte [criar a sua primeira base de dados SQL do Azure](sql-database-get-started-portal.md).

2. Ligar à base de dados com o SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Copiar o [script dentro da memória OLTP Transact-SQL](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) para sua área de transferência. O script T-SQL cria os objetos necessários dentro da memória da base de dados de exemplo AdventureWorksLT que criou no passo 1.

4. Cole o script T-SQL no SSMS e, em seguida, execute o script. O `MEMORY_OPTIMIZED = ON` instruções CREATE TABLE de cláusula são cruciais. Por exemplo:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Erro 40536


Se obtiver o erro 40536 ao executar o script T-SQL, execute o seguinte script de T-SQL para verificar se a base de dados suporta dentro da memória:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


O resultado de **0** significa que essa memória não é suportada, e **1** significa que é suportado. Para diagnosticar o problema, certifique-se de que a base de dados está na camada de serviço Premium.


#### <a name="about-the-created-memory-optimized-items"></a>Sobre os itens criados com otimização de memória

**Tabelas**: O exemplo contém as seguintes tabelas com otimização de memória:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Pode inspecionar a tabelas com otimização de memória através do **Object Explorer** no SSMS. Com o botão direito **tabelas** > **filtro** > **definições do filtro** > **tem otimizada de memória**. O valor é igual a 1.


Ou pode consultar as exibições de catálogo, tais como:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Procedimento armazenado compilado nativamente**: pode inspecionar SalesLT.usp_InsertSalesOrder_inmem através de uma consulta de vista de catálogo:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Executar a carga de trabalho do OLTP de exemplo

A única diferença entre os dois *procedimentos armazenados* é que o primeiro procedimento utiliza versões com otimização de memória das tabelas, enquanto o segundo procedimento utiliza as tabelas no disco regulares:

- SalesLT **.** usp_InsertSalesOrder **_inmem**
- SalesLT **.** usp_InsertSalesOrder **_ondisk**


Nesta secção, verá como utilizar o útil **ostress.exe** utilitário para executar os dois procedimentos armazenados em níveis estressante. Pode comparar o tempo que demora para execuções de duas estresse concluir.


Quando executa ostress.exe, recomendamos que passe os valores de parâmetro concebidos para os seguintes elementos:

- Executar um grande número de ligações em simultâneo, através da utilização - n100.
- Ter cada loop de ligação de centenas de vezes, ao utilizar - r500.


No entanto, convém começar com valores muito menores, como - n10 e - r50 para se certificar de que tudo está funcionando.


### <a name="script-for-ostressexe"></a>Script para ostress.exe


Esta secção apresenta o script de T-SQL que é incorporado na nossa linha de comando ostress.exe. O script utiliza os itens que foram criados pelo script T-SQL que instalou anteriormente.


O script a seguir insere uma ordem de venda de exemplo com cinco itens de linha para o seguinte com otimização de memória *tabelas*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Para tornar o *_ondisk* versão do script T-SQL anterior para ostress.exe, poderia substituir ambas as ocorrências da *_inmem* subcadeia com *_ondisk*. Estas substituições afetam os nomes das tabelas e procedimentos armazenados.


### <a name="install-rml-utilities-and-ostress"></a>Instale os utilitários RML e ostress


Idealmente, que teria que pretende executar ostress.exe numa máquina virtual do Azure (VM). Criaria uma [VM do Azure](https://azure.microsoft.com/documentation/services/virtual-machines/) na mesma região geográfica do Azure onde reside a sua base de dados do AdventureWorksLT. Mas pode executar em vez disso ostress.exe no seu computador portátil.


Na VM, ou em qualquer anfitrião escolher, instale os utilitários de linguagem de marcação de repetição (RML). Os utilitários incluem ostress.exe.

Para obter mais informações, consulte:
- A discussão ostress.exe [base de dados de exemplo para OLTP dentro da memória](http://msdn.microsoft.com/library/mt465764.aspx).
- [Base de dados de exemplo para OLTP dentro da memória](http://msdn.microsoft.com/library/mt465764.aspx).
- O [blogue para a instalação ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Executar o *_inmem* enfatizar primeiro a carga de trabalho


Pode utilizar um *linha de comandos do RML Cmd* janela para executar nossa linha de comandos ostress.exe. Os parâmetros da linha de comandos direta ostress para:

- Executar 100 ligações em simultâneo (-n100).
- Tem de cada ligação, execute o script T-SQL 50 vezes (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Para executar a linha de comando ostress.exe anterior:


1. Repor o conteúdo de dados de base de dados ao executar o seguinte comando no SSMS, para eliminar todos os dados que foi inseridos por quaisquer execuções anteriores:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Copie o texto da linha de comandos de ostress.exe anterior para a área de transferência.

3. Substitua o `<placeholders>` para os parâmetros -S - U+0b95 -P -d com os valores reais corretos.

4. Execute a linha de comandos editada numa janela RML Cmd.


#### <a name="result-is-a-duration"></a>Resultado é um período de tempo


Quando concluir o ostress.exe, escreve a duração da execução como a linha final de saída na janela RML Cmd. Por exemplo, uma execução de teste mais curta duração de aproximadamente 1,5 minutos:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Repor, editar para *_ondisk*, em seguida, volte a executar


Depois de ter o resultado a partir do *_inmem* executar, execute os seguintes passos para o *_ondisk* executar:


1. Repor a base de dados ao executar o seguinte comando no SSMS para eliminar todos os dados que é inserido através da execução anterior:
```
EXECUTE Demo.usp_DemoReset;
```

2. Edite a linha de comando ostress.exe para substituir todas *_inmem* com *_ondisk*.

3. Volte a executar ostress.exe pela segunda vez e capturar o resultado de duração.

4. Novamente, repor a base de dados (com responsabilidade a eliminar o que pode ser uma grande quantidade de dados de teste).


#### <a name="expected-comparison-results"></a>Resultados da comparação esperado

Nossos testes dentro da memória têm mostrado que desempenho melhorado **nove vezes** para esta carga de trabalho simplista, com ostress em execução numa VM do Azure na mesma região do Azure como a base de dados.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Instalar o exemplo de análise dentro da memória


Nesta secção, compare os resultados de e/s e estatísticas de quando estiver usando um índice columnstore em comparação com um índice de árvore b tradicional.


Para efetuar análises em tempo real numa carga de trabalho OLTP, muitas vezes é melhor usar um índice columnstore não em cluster. Para obter detalhes, consulte [descrito de índices Columnstore](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Preparar o teste de análise columnstore


1. Utilize o portal do Azure para criar uma nova base de dados do AdventureWorksLT do exemplo.
 - Utilize esse nome exato.
 - Escolha qualquer escalão de serviço Premium.

2. Copiar o [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) para sua área de transferência.
 - O script T-SQL cria os objetos necessários dentro da memória da base de dados de exemplo AdventureWorksLT que criou no passo 1.
 - O script cria duas tabelas de fatos e de tabela de dimensão. As tabelas de fatos são preenchidas com linhas de 3,5 milhões cada.
 - O script poderá demorar 15 minutos a concluir.

3. Cole o script T-SQL no SSMS e, em seguida, execute o script. O **COLUMNSTORE** palavra-chave no **CREATE INDEX** instrução é crucial, como em:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Definir o AdventureWorksLT para o nível de compatibilidade 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Nível 130 não está diretamente relacionado a funcionalidades de dentro da memória. Mas o nível 130 geralmente fornece desempenho de consulta mais rápido que 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Principais tabelas e índices columnstore


- dbo. FactResellerSalesXL_CCI é uma tabela que tem um índice columnstore em cluster, o que aumentou a compactação na *dados* nível.

- dbo. FactResellerSalesXL_PageCompressed é uma tabela que tem um equivalente índice em cluster regular, que é comprimido apenas nos *página* nível.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Consultas principais para comparar o índice columnstore


Existem [vários tipos de consultas de T-SQL que pode executar](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) para ver as melhorias de desempenho. Passo 2 no script T-SQL, preste atenção para esse par de consultas. Eles diferem apenas numa linha:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Um índice columnstore em cluster está no FactResellerSalesXL\_tabela CCI.

O seguinte trecho de script de T-SQL imprime as estatísticas de e/s e tempo para a consulta de cada tabela.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

Numa base de dados com o escalão de preço P2, pode esperar cerca de nove vezes o ganho de desempenho para esta consulta ao utilizar o índice columnstore em cluster em comparação com o índice tradicional. Com P15, pode esperar cerca de 57 vezes o ganho de desempenho ao utilizar o índice columnstore.



## <a name="next-steps"></a>Passos Seguintes

- [1 de início rápido: Tecnologias OLTP na memória para acelerar o desempenho de T-SQL](http://msdn.microsoft.com/library/mt694156.aspx)

- [Utilize o OLTP na memória num aplicativo existente do SQL do Azure](sql-database-in-memory-oltp-migration.md)

- [Armazenamento OLTP dentro da memória de monitor](sql-database-in-memory-oltp-monitoring.md) para OLTP dentro da memória


## <a name="additional-resources"></a>Recursos adicionais

#### <a name="deeper-information"></a>Informações mais detalhadas

- [Saiba como o quórum duplica a carga de trabalho de chave da base de dados, reduzindo assim a DTU por 70% com o OLTP dentro da memória na base de dados SQL](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [OLTP dentro da memória na mensagem de blogue de base de dados SQL do Azure](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Saiba mais sobre o OLTP dentro da memória](http://msdn.microsoft.com/library/dn133186.aspx)

- [Saiba mais sobre os índices columnstore](https://msdn.microsoft.com/library/gg492088.aspx)

- [Saiba mais sobre análise operacional em tempo real](http://msdn.microsoft.com/library/dn817827.aspx)

- Ver [padrões de carga de trabalho comuns e considerações sobre a migração](http://msdn.microsoft.com/library/dn673538.aspx) (que descreve padrões de carga de trabalho onde o OLTP dentro da memória fornece mais ganhos significativos de desempenho)

#### <a name="application-design"></a>Design do aplicativo

- [(Otimização de memória) de OLTP na memória](http://msdn.microsoft.com/library/dn133186.aspx)

- [Utilize o OLTP na memória num aplicativo existente do SQL do Azure](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Ferramentas

- [Portal do Azure](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
