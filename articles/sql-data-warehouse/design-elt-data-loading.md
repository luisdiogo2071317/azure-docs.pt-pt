---
title: Em vez de ETL, estruturar o ELT para o Azure SQL Data Warehouse | Documentos da Microsoft
description: Em vez de ETL, crie um processo de extração, carregamento e transformação (ELT) para carregar os dados ou o Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: d004ad1f24448da0c7404761ca0865826b3000b3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261286"
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Conceber a extração, carregamento e transformação (ELT) para o armazém de dados SQL do Azure

Em vez de extração, transformação e carregamento (ETL), crie um processo de extração, carregamento e transformação (ELT) para carregar dados para o Azure SQL Data Warehouse. Este artigo apresenta as formas de criar um processo ELT que move os dados para um armazém de dados do Azure.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]

## <a name="what-is-elt"></a>O que é o ELT?

Extrair, carga, e transformação (ELT) é um processo pelo qual os dados migram de um sistema de origem para um armazém de dados de destino. Este processo é efetuado regularmente, por exemplo por hora ou diariamente, para obter recentemente geradas dados no armazém de dados. A melhor forma de obter dados de origem para o armazém de dados é desenvolver um processo ELT que utiliza o PolyBase para carregar dados para o SQL Data Warehouse.

ELT for carregado primeiro e, em seguida, transforma os dados, ao passo que a extração, transformação e carregamento (ETL) transforma os dados antes de carregá-lo. Execução de ELT, em vez de ETL poupa no custo de fornecer seus próprios recursos para transformar os dados antes de ele é carregado. Ao utilizar o SQL Data Warehouse, ELT tira partido do sistema MPP para efetuar as transformações.

Embora existam muitas variações para a implementação de ELT para o SQL Data Warehouse, estas são as etapas básicas:  

1. Extrair a origem de dados em ficheiros de texto.
2. Direcionado para os dados no armazenamento de Blobs do Azure ou do Azure Data Lake Store.
3. Prepare os dados para carregamento.
2. Carregar os dados para tabelas de testes com o PolyBase do SQL Data Warehouse.
3. Transforme os dados.
4. Inserir os dados em tabelas de produção.


Para obter um tutorial de carregamento, veja [utilizar o PolyBase para carregar dados do armazenamento de Blobs do Azure ao Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Para obter mais informações, consulte [blog de padrões de carregamento](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Opções de carregamento com o PolyBase

O PolyBase é uma tecnologia que acessa dados fora da base de dados por meio de linguagem T-SQL. É a melhor forma de carregar dados para o SQL Data Warehouse. Com o PolyBase, o carregamento dos dados em paralelo da origem de dados diretamente para os nós de computação. 

Para carregar dados com o PolyBase, pode usar qualquer uma destas opções de carregamento.

- [O PolyBase com T-SQL](load-data-from-azure-blob-storage-using-polybase.md) funciona bem quando os seus dados estão no armazenamento de Blobs do Azure ou no Azure Data Lake Store. Dá-lhe mais controle sobre o processo de carregamento, mas também requer que definir os objetos de dados externa. Os outros métodos definem esses objetos em segundo plano, como mapear tabelas de origem para tabelas de destino.  Para orquestrar a cargas de T-SQL, pode utilizar o Azure Data Factory, SSIS ou as funções do Azure. 
- [O PolyBase com o SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) funciona bem quando os dados de origem estão no SQL Server, SQL Server no local ou na cloud. SSIS define a origem para mapeamentos de tabela de destino e também orquestra a carga. Se já tiver pacotes do SSIS, pode modificar os pacotes para trabalhar com o novo destino de armazém de dados. 
- [O PolyBase com o Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) é outra ferramenta de orquestração.  Ele define um pipeline e agenda trabalhos. 
- [O PolyBase com o Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) transfere dados a partir de uma tabela do SQL Data Warehouse para um dataframe do Databricks e/ou escreve dados de um pacote de dados do Databricks a uma tabela do SQL Data Warehouse.

### <a name="polybase-external-file-formats"></a>Formatos de ficheiro externo de PolyBase

O PolyBase carrega dados de UTF-8 e UTF-16 codificado de ficheiros de texto delimitados. Além dos ficheiros de texto delimitado, ele carrega dos formatos de ficheiro Hadoop RC ficheiro ORC e no Parquet. O PolyBase pode carregar dados do Gzip e arquivos compactados Snappy. O PolyBase atualmente não suporta ASCII estendido, o formato de largura fixa e formatos aninhados, como o WinZip, JSON e XML.

### <a name="non-polybase-loading-options"></a>Opções de carregamento PolyBase não
Se seus dados não são compatíveis com o PolyBase, pode utilizar [bcp](/sql/tools/bcp-utility) ou o [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP carrega diretamente ao SQL Data Warehouse sem passar pelo armazenamento de Blobs do Azure e destina-se apenas para pequenas cargas. Tenha em atenção de que o desempenho de carga dessas opções é significativamente mais lento do que o PolyBase. 


## <a name="extract-source-data"></a>Extrair dados de origem

Obter dados fora do seu sistema de origem depende da origem.  O objetivo é mover os dados em arquivos de texto delimitado. Se estiver a utilizar o SQL Server, pode utilizar [ferramenta da linha de comandos do bcp](/sql/tools/bcp-utility) para exportar os dados.  

## <a name="land-data-to-azure-storage"></a>Mover dados para o armazenamento do Azure

Parar os dados no armazenamento do Azure, pode movê-lo para [armazenamento de Blobs do Azure](../storage/blobs/storage-blobs-introduction.md) ou [do Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). Em ambos os locais, os dados devem ser armazenados em ficheiros de texto. O Polybase pode carregar a partir de ambos os locais.

Estes são ferramentas e serviços que pode utilizar para mover dados para o armazenamento do Azure.

- [O Azure ExpressRoute](../expressroute/expressroute-introduction.md) serviço melhora o débito de rede, desempenho e previsibilidade. O ExpressRoute é um serviço que encaminha seus dados através de uma ligação privada dedicada para o Azure. As ligações ExpressRoute não encaminhar os dados através da internet pública. As ligações oferecem mais fiabilidade, velocidades mais rápidas, latências mais baixas e maior segurança do que as ligações normais pela internet pública.
- [O utilitário AZCopy](../storage/common/storage-moving-data.md) move os dados ao armazenamento do Azure através da internet pública. Isso funciona se os tamanhos de dados for inferior a 10 TB. Para executar cargas de forma regular, com o AZCopy, teste a velocidade da rede para ver se é aceitável. 
- [O Azure Data Factory (ADF)](../data-factory/introduction.md) tem um gateway que pode instalar no seu servidor local. Em seguida, pode criar um pipeline para mover dados do seu servidor local até o armazenamento do Azure. Para utilizar o Data Factory com o SQL Data Warehouse, consulte [carregar dados para o SQL Data Warehouse](/azure/data-factory/load-azure-sql-data-warehouse).

## <a name="prepare-data"></a>Preparar dados

Poderá ter de preparar e limpar os dados na sua conta de armazenamento antes de os carregar para o SQL Data Warehouse. Preparação de dados pode ser executada enquanto os seus dados estão na fonte, como exportar os dados em arquivos de texto, ou depois dos dados no armazenamento do Azure.  É mais fácil de trabalhar com os dados mais cedo no processo de possível.  

### <a name="define-external-tables"></a>Definir tabelas externas
Antes de carregar dados, terá de definir tabelas externas no seu armazém de dados. O PolyBase utiliza tabelas externas para definir e aceder aos dados no armazenamento do Azure. A tabela externa é semelhante a uma tabela normal. A principal diferença é que os pontos de tabela externa para os dados armazenados fora do armazém de dados. 

Definir tabelas externas envolve a especificação da origem de dados, o formato dos ficheiros de texto e as definições de tabela. Estes são os tópicos de sintaxe de T-SQL que será necessário:
- [CRIAR ORIGEM DE DADOS EXTERNA](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CRIAR TABELA EXTERNA](/sql/t-sql/statements/create-external-table-transact-sql)

Para obter um exemplo de criação de objetos externos, consulte a [criar tabelas externas](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) passo do tutorial de carregamento.

### <a name="format-text-files"></a>Ficheiros de texto de formato

Assim que são definidos os objetos externos, tem de alinhar as linhas dos ficheiros de texto com a tabela externa e a definição de formato de ficheiro. Os dados em cada linha do arquivo de texto devem estar alinhadas com a definição da tabela.

Para formatar os arquivos de texto:

- Se seus dados é proveniente de uma origem de não-relacionais, terá de transformá-lo em linhas e colunas. Se os dados estão a partir de uma origem relacional ou não relacionais, os dados precisam ser transformados para alinhar com as definições da coluna para a tabela na qual pretende carregar os dados. 
- Formatar dados no arquivo de texto para alinhar com os colunas e tipos de dados da tabela de destino do SQL Data Warehouse. Alinhamento incorreto entre tipos de dados nos arquivos de texto externos e a tabela de armazém de dados faz com que as linhas para serem rejeitadas durante o carregamento.
- Campos separados no arquivo de texto com um terminador.  Certifique-se de que utiliza um caráter ou uma seqüência de caracteres que não se encontra nos seus dados de origem. Utilizar o terminador especificado com [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Carregar para uma tabela de teste
Para obter dados no armazém de dados, ele funciona bem para o primeiro carregamento os dados para uma tabela de testes. Ao utilizar uma tabela de teste, pode lidar com erros sem interferir com as tabelas de produção e evitar a execução de operações de reversão na tabela de produção. Uma tabela de teste também lhe dá a oportunidade para utilizar o SQL Data Warehouse para executar transformações antes de inserir os dados em tabelas de produção.

Para carregar com o T-SQL, execute o [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) instrução T-SQL. Este comando insere os resultados de uma instrução select para uma nova tabela. Quando seleciona a declaração de uma tabela externa, ela importa os dados externos. 

No exemplo a seguir, ext. A data é uma tabela externa. Todas as linhas são importadas para uma nova tabela chamada dbo. Data.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Transformar os dados
Embora seja dados na tabela de teste, efetue transformações que precisa de sua carga de trabalho. Em seguida, mova os dados na tabela de produção.

## <a name="insert-data-into-production-table"></a>Inserir dados na tabela de produção

A inserção em... Instrução SELECT move os dados da tabela de teste para a tabela permanente. 

À medida que concebe um processo de ETL, tente executar o processo de uma amostra de teste pequeno. Tente extrair 1000 linhas da tabela para um ficheiro, movê-lo para o Azure e, em seguida, tente carregar para uma tabela de teste. 

## <a name="partner-loading-solutions"></a>Soluções de carregamento de parceiros
Muitos dos nossos parceiros de ter a carregar soluções. Para obter mais informações, consulte uma lista de nosso [parceiros de solução](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Passos Seguintes
Para carregar a documentação de orientação, veja [documentação de orientação para carregar dados](guidance-for-loading-data.md).


