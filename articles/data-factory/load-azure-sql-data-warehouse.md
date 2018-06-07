---
title: Carregar dados para o Azure SQL Data Warehouse, utilizando o Azure Data Factory | Microsoft Docs
description: Utilizar o Azure Data Factory para copiar dados para o Azure SQL Data Warehouse
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: c7549297f040e251f3c0109debf757c28750d0a0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619274"
---
# <a name="load-data-into-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Carregar dados para o Azure SQL Data Warehouse, utilizando o Azure Data Factory

[O Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) é a base de dados baseado na nuvem, de escalamento horizontal que seja capaz de processar grandes volumes de dados tanto relacionais como não relacionais. O SQL Data Warehouse baseia-se a arquitetura de processamento paralelo em grande escala (MPP) que está otimizado para cargas de trabalho de armazém de dados de empresa. Oferece elasticidade da nuvem com a flexibilidade ao dimensionar o armazenamento e computação de forma independente.

Introdução ao Azure SQL Data Warehouse agora é mais fácil do que nunca ao utilizar Azure Data Factory. O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerido. Pode utilizar o serviço para preencher um SQL Data Warehouse com dados do sistema existente e poupar tempo ao criar as suas soluções de análise.

O Azure Data Factory oferece as seguintes vantagens para carregar dados para o Azure SQL Data Warehouse:

* **Fácil de configurar**: um Assistente de passo 5 intuitivo com nenhuma scripting necessário.
* **Suporte de arquivo de dados avançados**: suporte incorporado para um conjunto avançado de arquivos de dados baseado na nuvem e no local. Para obter uma lista detalhada, consulte a tabela de [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
* **Segura e compatível**: os dados são transferidos através de HTTPS ou ExpressRoute. A presença de serviço global garante que os dados nunca sai do limite geográfico.
* **Desempenho unparalleled utilizando o PolyBase**: o Polybase é a forma mais eficiente para mover dados para o Azure SQL Data Warehouse. Utilize a funcionalidade de blob transição para alcançar a elevada carga velocidades de todos os tipos de arquivos de dados, incluindo o armazenamento de Blobs do Azure e a Data Lake Store. (O Polybase suporta Blob storage do Azure e o Azure Data Lake Store por predefinição.) Para obter mais informações, consulte [copiar o desempenho de atividade](copy-activity-performance.md).

Este artigo mostra-lhe como utilizar a ferramenta de dados de cópia de fábrica de dados para _carregar dados do SQL Database do Azure para o Azure SQL Data Warehouse_. Pode seguir passos semelhantes para copiar dados de outros tipos de arquivos de dados.

> [!NOTE]
> Para obter mais informações, consulte [copiar dados de ou para o Azure SQL Data Warehouse, utilizando o Azure Data Factory](connector-azure-sql-data-warehouse.md).
>
> Este artigo aplica-se à versão 2 do Azure Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte o artigo [atividade de cópia no Azure Data Factory versão 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure: Se não tiver uma subscrição do Azure, crie um [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* O Azure SQL Data Warehouse: O armazém de dados retém os dados que são copiados através da base de dados do SQL Server. Se não tiver um armazém de dados SQL do Azure, consulte as instruções no [criar um SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md).
* Base de dados SQL do Azure: Neste tutorial copia dados a partir de uma base de dados SQL do Azure com dados de exemplo Adventure Works LT. Pode criar uma base de dados do SQL Server ao seguir as instruções em [criar uma base de dados SQL do Azure](../sql-database/sql-database-get-started-portal.md). 
* Conta de armazenamento do Azure: armazenamento do Azure é utilizado como o _transição_ blob na operação de cópia em massa. Se não tem uma conta de armazenamento do Azure, veja as instruções apresentadas em [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. No menu da esquerda, selecione **novo** > **dados + análise** > **Data Factory**: 
   
   ![Criar uma nova fábrica de dados](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** página, forneça valores para os campos que são apresentados na imagem seguinte:
      
   ![Página Nova fábrica de dados](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
    * **Nome**: introduza um nome globalmente exclusivo para a fábrica de dados do Azure. Se receber o erro "o nome de factory de dados \"LoadSQLDWDemo\" não está disponível," Introduza um nome diferente para a fábrica de dados. Por exemplo, pode utilizar o nome  _**yourname**_**ADFTutorialDataFactory**. Tente criar a fábrica de dados novamente. Para ter acesso às regras de nomenclatura para artefactos do Data Factory, veja [Regras de nomenclatura do Data Factory](naming-rules.md).
    * **Subscrição**: selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados. 
    * **Grupo de recursos**: selecione um grupo de recursos existente na lista pendente, ou selecione o **criar nova** opção e introduza o nome de um grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versão**: selecione **V2 (pré-visualização)**.
    * **Localização**: selecione a localização da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Arquivos de dados que são utilizados pela fábrica de dados podem estar nas outras regiões e localizações. Estes arquivos de dados incluem o Azure Data Lake Store, o Storage do Azure, SQL Database do Azure e assim sucessivamente.

3. Selecione **Criar**.
4. Depois de concluída a criação, vá para a fábrica de dados. Pode ver o **Data Factory** home page do conforme mostrado na imagem seguinte:
   
   ![Home page da fábrica de dados](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

   Selecione o **autor & Monitor** mosaico para iniciar a aplicação de integração de dados num separador separado.

## <a name="load-data-into-azure-sql-data-warehouse"></a>Carregar dados para o Azure SQL Data Warehouse

1. No **começar** página, selecione o **copiar dados** mosaico para iniciar a ferramenta de cópia de dados:

   ![Mosaico ferramenta Copiar Dados](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. No **propriedades** página, especifique **CopyFromSQLToSQLDW** para o **nome da tarefa** campo e selecione **seguinte**:

    ![Página Propriedades](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. No **arquivo de dados de origem** página, selecione **SQL Database do Azure**e selecione **seguinte**:

    ![Página de arquivo de dados de origem](./media/load-azure-sql-data-warehouse/specify-source.png)
4. Na página **Especificar base de dados SQL do Azure**, execute os seguintes passos: 
   1. Em **Nome do servidor**, selecione o seu servidor do SQL do Azure.
   2. Em **Nome da base de dados**, selecione a sua base de dados SQL do Azure.
   3. Especifique o nome do utilizador para o **Nome de utilizador**.
   4. Especifique palavra-passe o utilizador **palavra-passe**.
   5. Selecione **Seguinte**.
   
   ![Especifique a base de dados SQL do Azure](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. No **selecione tabelas a partir da qual pretende copiar os dados ou utilizar uma consulta personalizada** página, introduza **SalesLT** para filtrar as tabelas. Escolha o **(Selecionar tudo)** caixa para utilizar todas as tabelas para a cópia e, em seguida, selecione **seguinte**: 

    ![Selecione tabelas de origem](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. No **arquivo de dados de destino** página, selecione **Azure SQL Data Warehouse**e selecione **seguinte**:

    ![Página arquivo de dados de destino](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. No **especificar o Azure SQL Data Warehouse** página, efetue os seguintes passos: 

   1. Em **Nome do servidor**, selecione o seu servidor do SQL do Azure.
   2. Selecione o seu armazém de dados SQL do Azure para o **nome de base de dados**.
   3. Especifique o nome do utilizador para o **Nome de utilizador**.
   4. Especifique palavra-passe o utilizador **palavra-passe**.
   5. Selecione **Seguinte**.
   
   ![Especifique o armazém de dados SQL do Azure](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. No **mapeamento de tabela** página, reveja o conteúdo e selecione **seguinte**. Apresenta um mapeamento de tabela inteligente. As tabelas de origem estão mapeadas para as tabelas de destino com base nos nomes de tabela. Se uma tabela de origem não existe no destino do Azure Data Factory cria uma tabela de destino com o mesmo nome, por predefinição. Também pode mapear uma tabela de origem para uma tabela de destino existente. 

   > [!NOTE]
   > Criação automática de tabela para o sink do SQL Data Warehouse aplica-se quando o SQL Server ou SQL Database do Azure é a origem. Se copiar dados a partir de outro arquivo de dados de origem, terá de pré-criar o esquema no receptor de Azure SQL Data Warehouse antes de executar a cópia de dados.

   ![Página de mapeamento de tabela](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. No **mapeamento de esquema** página, reveja o conteúdo e selecione **seguinte**. O mapeamento de tabela inteligente é baseado no nome da coluna. Se permitem criar automaticamente as tabelas de fábrica de dados, a conversão do tipo de dados pode ocorrer quando existem incompatibilidades entre os arquivos de origem e de destino. Se houver uma conversão de tipo de dados não suportados entre a coluna de origem e de destino, verá uma mensagem de erro junto a tabela correspondente.

    ![Página de mapeamento de esquema](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. No **definições** página, selecione a conta de armazenamento do Azure no **nome da conta de armazenamento** na lista pendente. A conta é utilizada para sincronizar os dados antes de medida que é carregado para o SQL Data Warehouse, utilizando o PolyBase. Depois da cópia estiver concluída, os dados provisórias no armazenamento do Azure é automaticamente limpa. Em **definições avançadas**, desmarque a **utilizar predefinição do tipo** opção:

    ![Página de definições](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. No **resumo** página, reveja as definições e selecione **seguinte**:

    ![Página de resumo](./media/load-azure-sql-data-warehouse/summary-page.png)
13. No **página implementação**, selecione **Monitor** para monitorizar o pipeline (tarefas):

    ![Página de implementação](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Tenha em atenção que o separador **Monitorização** à esquerda é selecionado automaticamente. O **ações** coluna inclui ligações para ver os detalhes de execução da atividade e volte a executar o pipeline: 

    ![Monitorizar execuções de pipeline](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Para ver as execuções de atividade que estão associadas com o pipeline de execução, selecione o **ver a atividade é executada** ligação no **ações** coluna. Existem 10 atividades de cópia no pipeline e cada atividade copia uma tabela de dados. Para mudar para o pipeline é executado vista, selecione o **Pipelines** ligação na parte superior. Selecione **Atualizar** para atualizar a lista. 

    ![Monitorização de execuções de atividade](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Para monitorizar os detalhes de execução para cada atividade de cópia, selecione o **detalhes** ligação em **ações** na atividade de vista de monitorização. Pode monitorizar detalhes como o volume de dados copiado da origem para o sink, débito de dados, os passos de execução com duração correspondente e utilizado configurações:

    ![Detalhes da execução de atividade de monitorização](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Passos Seguintes

Avançar para o artigo seguinte para saber mais sobre o suporte do Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Conector do Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md)
