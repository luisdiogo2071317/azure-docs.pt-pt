---
title: Carregar dados para o Azure SQL Data Warehouse com o Azure Data Factory | Microsoft Docs
description: Utilizar o Azure Data Factory para copiar dados para o Azure SQL Data Warehouse
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Carregar dados para o Azure SQL Data Warehouse com o Azure Data Factory

[O Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) é uma base de dados horizontal, baseada na nuvem, capaz de processar grandes volumes de dados tanto relacionais como não relacionais.  Baseado na arquitetura de processamento paralelo em grande escala (MPP), o SQL Data Warehouse está otimizado para cargas de trabalho de armazém de dados de empresa.  Oferece elasticidade da nuvem com a flexibilidade ao dimensionar o armazenamento e computação de forma independente.

Introdução ao Azure SQL Data Warehouse agora é mais fácil do que nunca utilizando **do Azure Data Factory**.  O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerido, que pode ser utilizado para preencher um SQL Data Warehouse com os dados do sistema existente e guardar a importantes tempo ao avaliar o SQL Data Warehouse e criar as suas soluções de análise. Seguem-se as principais vantagens de carregamento dos dados no Azure SQL Data Warehouse, utilizando o Azure Data Factory:

* **Fácil de configurar**: Assistente intuitivo do passo 5 com nenhuma scripting necessário.
* **Suporte de arquivo de dados avançados**: suporte incorporado para um conjunto avançado de no local e arquivos de dados baseado na nuvem, consulte uma lista detalhada no [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.
* **Segura e compatível**: os dados são transferidos através de HTTPS ou ExpressRoute e presença global serviço assegura que os dados nunca sai do limite geográfico
* **Desempenho unparalleled utilizando o PolyBase**: utilizando o Polybase é a forma mais eficiente para mover dados para o Azure SQL Data Warehouse. Utilizar a funcionalidade de blob de teste, pode conseguir velocidades de carga elevada de todos os tipos de arquivos de dados para além do Blob storage do Azure e a Data Lake Store, o Polybase suporta por predefinição. Saiba os detalhes da [copiar o desempenho de atividade](copy-activity-performance.md).

Este artigo mostra como utilizar a ferramenta de dados de cópia de fábrica de dados para **carregar dados do SQL Database do Azure para o Azure SQL Data Warehouse**. Pode seguir os passos semelhantes para copiar dados de outros tipos de arquivos de dados.

> [!NOTE]
>  Para obter informações gerais sobre as capacidades de fábrica de dados ao copiar dados da Azure SQL Data Warehouse, consulte [copiar dados de ou para o Azure SQL Data Warehouse, utilizando o Azure Data Factory](connector-azure-sql-data-warehouse.md) artigo.
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Pré-requisitos

* **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.
* **Azure SQL Data Warehouse**. Este armazém de dados contém os dados copiados da Base de Dados SQL. Se não tiver um Azure SQL Data Warehouse, veja o artigo [Criar um SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) para obter os passos para criar um.
* **Base de Dados SQL do Azure**. Este tutorial copia dados a partir de uma base de dados do SQL do Azure com dados de exemplo Adventure Works LT, pode criar um seguinte o [criar uma base de dados SQL do Azure](../sql-database/sql-database-get-started-portal.md) artigo. 
* **Conta de Armazenamento do Azure**. Storage do Azure é utilizado como **transição** blob na operação de cópia em massa. Se não tiver uma conta de armazenamento do Azure, veja o artigo [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account) para obter os passos para criar uma.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **NOVO**, no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** , indique os valores, conforme mostrado na captura de ecrã seguinte:
      
     ![Nova página da fábrica de dados](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Name.** Introduza um nome exclusivo global para a fábrica de dados. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory) e tente criá-la novamente. Consulte [fábrica de dados - as regras de nomenclatura](naming-rules.md) artigo para regras de nomenclatura artefactos do Data Factory de dados.
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Subscrição.** Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
    * **Grupo de recursos.** Selecione um grupo de recursos existente na lista pendente, ou selecione **criar nova** opção e introduza o nome de um grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versão.** Selecione **V2 (pré-visualização)**.
    * **Localização.** Selecione a localização da fábrica de dados. Apenas as localizações suportadas são apresentadas na lista pendente. Arquivos de dados (Storage do Azure, SQL Database do Azure, etc.) utilizados pela fábrica de dados podem ser noutras localizações/regiões. 

3. Clique em **Criar**.
4. Após a criação for concluída, aceda à fábrica de dados e ver o **Data Factory** página conforme mostrado na imagem. Clique em **autor & Monitor** mosaico para iniciar a aplicação de integração de dados num separador separado.
   
   ![Home page da fábrica de dados](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Carregar dados para o Azure SQL Data Warehouse

1. Na página de introdução de get, clique em **copiar dados** mosaico para iniciar a ferramenta de dados de cópia. 

   ![Mosaico de ferramenta de dados de cópia](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. No **propriedades** página da ferramenta de copiar os dados, especifique **CopyFromSQLToSQLDW** para o **nome da tarefa**e clique em **seguinte**. 

    ![Copie a página de propriedades da ferramenta de dados](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. No **arquivo de dados de origem** página, selecione **SQL Database do Azure**e clique em **seguinte**.

    ![Página de arquivo de dados de origem](./media/load-azure-sql-data-warehouse/specify-source.png)
4. No **especificar a base de dados SQL do Azure** página, efetue os seguintes passos: 
    1. Selecione o seu servidor SQL do Azure para o **nome do servidor**.
    2. Selecione a base de dados SQL do Azure para o **nome de base de dados**.
    3. Especifique o nome do utilizador para **nome de utilizador**.
    4. Especifique a palavra-passe do utilizador para **palavra-passe**.
    5. Clique em **Seguinte**. 

        ![Especifique a base de dados SQL do Azure](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. No **selecione tabelas a partir da qual pretende copiar os dados ou utilizar uma consulta personalizada** página, filtrar as tabelas, especificando **SalesLT** na caixa de entrada, em seguida, verifique o **(Selecionar tudo)** caixa de verificação para selecionar todas as tabelas e, em seguida, clique em **seguinte**. 

    ![Escolha a pasta ou ficheiro de entrada](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. No **arquivo de dados de destino** página, selecione **Azure SQL Data Warehouse**e clique em **seguinte**. 

    ![Página de arquivo de dados de destino](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. No **especificar o Azure SQL Data Warehouse** página, efetue os seguintes passos: 

    1. Selecione o seu servidor SQL do Azure para o **nome do servidor**.
    2. Selecione o seu armazém de dados SQL do Azure para o **nome de base de dados**.
    3. Especifique o nome do utilizador para **nome de utilizador**.
    4. Especifique a palavra-passe do utilizador para **palavra-passe**.
    5. Clique em **Seguinte**. 

        ![Especifique o armazém de dados SQL do Azure](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. No **mapeamento de tabela** , reveja e, em **seguinte**. Um mapeamento de tabela inteligente aparece que mapas de origem com base nos nomes de tabela de tabelas de destino. Se a tabela não existe no destino, por predefinição do Azure Data Factory cria um com o mesmo nome. Também pode optar por mapear para uma tabela existente. 

    > [!NOTE]
    > Criação da tabela automática para sink do SQL Data Warehouse aplica-se quando o SQL Server ou SQL Database do Azure é a origem. Se copiar dados a partir de outro arquivo de dados de origem, é necessário para pré criar o esquema no sink do Azure SQL Data Warehouse antes de cópia de dados.

    ![Página de mapeamento de tabela](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. No **mapeamento de esquema** , reveja e, em **seguinte**. Mapeamento inteligente é baseado no nome da coluna. Se optar por permitir automática de fábrica de dados criar as tabelas, conversão de tipo de dados adequada pode ocorrer se for necessário para corrigir a incompatibilidade entre os arquivos de origem e destino. Se houver uma conversão de tipo de dados não suportados entre a coluna de origem e de destino, verá uma mensagem de erro juntamente com a tabela correspondente.

    ![Página de mapeamento de esquema](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. No **definições** página, selecione um Storage do Azure no **nome da conta de armazenamento** na lista pendente. É utilizado para sincronizar os dados antes de medida que é carregado para o SQL Data Warehouse, utilizando o PolyBase. Depois de terminar a cópia, os dados no armazenamento provisórias serão eliminados automaticamente. 

    Em "Definições avançadas", desmarque a opção "Utilizar predefinição do tipo".

    ![Página de definições](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. No **resumo** página, reveja as definições e clique em **seguinte**.

    ![Página de resumo](./media/load-azure-sql-data-warehouse/summary-page.png)
13. No **página implementação**, clique em **Monitor** para monitorizar o pipeline (tarefas).

    ![Página de implementação](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Tenha em atenção que o **Monitor** separador à esquerda é selecionado automaticamente. Pode ver as ligações para ver detalhes de execução da atividade e volte a executar o pipeline de **ações** coluna. 

    ![Monitorizar o pipeline é executada](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Para ver as execuções de atividade associadas com o pipeline executar, clique em **ver a atividade é executada** ligação no **ações** coluna. 10 atividades de cópia no pipeline, copia cada uma tabela de dados. Para mudar para o pipeline é executado vista, clique em **Pipelines** ligação na parte superior. Clique em **atualizar** para atualizar a lista. 

    ![Execuções de atividade do monitor](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Ainda pode monitorizar detalhes da execução de cada atividade de cópia, clicando a **detalhes** ligação em **ações** na vista de monitorização de atividade. Mostra informações, incluindo o volume dos dados copiados a partir da origem para sink, débito, os passos que realiza com duração correspondente e utilizado configurações.

    ![Detalhes da execução de atividade de monitorização](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Passos Seguintes

Avançar para o artigo seguinte para saber mais sobre o suporte do Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Conector do Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md)